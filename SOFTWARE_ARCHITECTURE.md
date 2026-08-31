# 💻 Software-Architektur & Deterministisches Echtzeit-Framework

Dieses Dokument dokumentiert die softwareseitige Architektur, die asynchrone Kommunikationsmatrix und die Echtzeit-Datenverarbeitung des SVA-Fencing-Testers zum Schutz des geistigen Eigentums (Stand der Technik). Der Fokus der Firmware liegt auf absolutem Determinismus, Lock-Free-Ressourceneffizienz und der Vermeidung von blockierendem Overhead zur Gewährleistung der 860-Hz-Echtzeit-Messtaktung auf der ESP32-S3-Plattform.

---

### 1. Striktes Task-Priorisierungs-Modell (RTOS & Bare-Metal Hybrid)
Die Firmware arbeitet auf Basis von FreeRTOS in Kombination mit hardwarenahen Interrupts (INT) und dedizierten Timern (Ticker). Zur Gewährleistung Jitter-freier Messungen sind die Systemaufgaben streng nach zeitlicher Kritikalität hierarchisch auf die CPU-Kerne aufgeteilt:
* **Priorität 1: Der Mess-Task (Core 0):** Höchste Systempriorität. Liest den I2C-Messwandler (ADS1115) im kontinuierlichen Modus aus, steuert die binäre FET-Kreuzmatrix und berechnet die ratiometrischen Widerstandswerte.
* **Priorität 2: Der UI-Task (Core 1):** Mittlere Priorität. Verwaltet die grafische Benutzeroberfläche des TFT-Displays (Touch-Eingaben, Kurven-Rendering aus dem PSRAM über den SPI-Bus).
* **Priorität 3: Der WiFi/Netzwerk-Task (Core 0/1):** Niedrigste Priorität. Verwaltet den HTTP-Server und die asynchrone Bereitstellung der JSON/Binär-Datenströme für die Webapp.

---

### 2. Lock-Free I2C-Zentralisierung & Thread-Sicherheit
Um blockierende Mutex- oder Semaphor-Sperren (Locks) auf dem I2C-Bus zu verhindern, die das Timing des zeitkritischen Mess-Tasks unvorhersehbar verzögern würden, sind sämtliche I2C-Zugriffe exklusiv im Mess-Task zentralisiert:
* **Flag-Steuerung:** Andere Tasks (z. B. UI oder Webapp) fordern Daten oder Einstellungsänderungen (wie das Schreiben in den FRAM oder das Auslesen der RTC) rein über atomare Status-Flags an. Der Mess-Task arbeitet diese Anforderungen sequenziell in seinen natürlichen Messpausen ab. Der Bus bleibt vollkommen kollisions- und blockierungsfrei.

---

### 3. Ultra-Schlanker Modulo-Ringpuffer (Lock-Free Circular Buffer)
Der Datenaustausch der hochfrequenten Messwerte zwischen Core 0 (Messung) und Core 1 (UI-Darstellung) erfolgt über einen maßgeschneiderten, hochoptimierten Ringpuffer, der ohne Mutex-Sperren auskommt:
* **Entkoppelte Pointer-Logik:** Der Producer (Mess-Task) inkrementiert ausschließlich den `Head`-Pointer. Der Consumer (UI-Task) verwaltet einen eigenen, isolierten `Tail`-Pointer.
* **Modulo- & Overflow-Handling:** Der physische Speicherindex wird über eine einfache Modulo-Operation mit der Pufferkapazität berechnet. Der `Head`-Pointer läuft als fortlaufender Ganzzahlwert ($N \times \text{Kapazität}$) endlos hoch. Dadurch wird das systemkritische Überlauf-Handling (Overflow) mathematisch trivial gelöst, die CPU-Zyklen pro Schreibvorgang auf ein absolutes Minimum reduziert und Race Conditions prinzipiell ausgeschlossen.

---

### 4. Proprietäres Binär-Datenformat zur Bandbreiten-Minimierung
Um den WLAN-Durchsatz und den RAM-Overhead des ESP32-S3 im 860-Hz-Betrieb minimal zu halten, werden Live-Messdaten nicht als Text (JSON), sondern in einem hochkomprimierten, proprietären **16-Bit-Binärformat** gestreamt:
* **2 Steuerbits (Bit 15–14):** Definieren den Kontext des Datenpakets (z. B. Kanal-ID, Statustyp oder Bereichs-Indikator).
* **14 Databits (Bit 13–0):** Übertragen den reinen Nutzwert.
* **Hybride Skalierung (Festkomma & Logarithmisch):** Im FIE-relevanten Niederohmbereich ($< 10\,\Omega$) arbeiten die 14 Bits als hochauflösendes Festkommaformat zur exakten Zehntel-Ohm-Darstellung. Wechselt das System in den hochohmigen Isolationsbereich ($> 500\,\text{k}\Omega$), schaltet das Format automatisch auf eine logarithmische Skalierung um. Dies garantiert ein Maximum an signifikanten Stellen (*Maximum Significants*) bei gleichzeitig maximaler Dynamikreichweite (*Maximum Range*) innerhalb eines einzigen 2-Byte-Wortes.

---

### 5. LittleFS-Partitionierung & Redundantes Dual-App-Speicherschema
Die Flash-Topologie des ESP32-S3 ist strikt in funktionale Segmente unterteilt, um die Speicherbandbreite optimal zu nutzen:
* **Duale App-Redundanz:** Der ausführbare Firmware-Kern arbeitet im klassischen Dual-Partition-Verfahren (app0 und app1). Dies ermöglicht sichere, fehlerresistente Over-the-Air-Updates des Core-Codes im laufenden Betrieb.
* **Isolierte Webapp- & Manual-Partition:** Die komprimierte Single-Page-Application (webapp.gz) sowie die digitalen Benutzerhandbücher (manuals) sind vollständig in eine separate, dedizierte LittleFS-Partition ausgelagert. Dies verhindert das Überschreiten der App-Speichergrenzen und entkoppelt Web-Ressourcen logisch vom Binärcode.

---

### 6. Unzerstörbares Werks-Dateisystem (FFat-Immutable-Partition)
Die Systemarchitektur nutzt eine dedizierte, verschleißfreie Flash-Dateisystempartition (FFat) zur Speicherung der unveränderlichen System-Assets. FIE-Werkspresets, Systemspezifikationen und Custom-Vereinslogos sind direkt in diesem partitionsbasierten Speichersegment verankert. Diese Immutable-Struktur ist fest an den OTA-Update-Pfad gekoppelt, wodurch ein unzerstörbarer System-Fallback (Factory Reset) bei Datenkorruption des externen flüchtigen Speichers systemisch garantiert ist.

---

### 7. Unified-Firmware mit I²C-Autoscan & Fallback-Logik
Die Firmware führt beim Systemstart einen automatischen Peripheriescan durch und adaptiert vorhandene Hardware-Topologien dynamisch zur Laufzeit:
* **Modulare Bestückungsvarianten:** Das System erkennt das Fehlen von RTC (Fallstand: keine Uhrzeit), sekundärem ADC (Wechsel von Parallelsampling auf sequenzielles Time-Multiplexing) oder FRAM (Wechsel auf EEPROM/Flash) und schaltet die Systemlogik verzögerungsfrei in den jeweiligen Fallback-Modus.
* **Vollständige Abwärtskompatibilität:** Durch die Abfrage historischer I²C-Adressen (z. B. 0x2F für Digital-Potentiometer oder 0x68 für ältere RTC-Typen) bleibt ein einziger, einheitlicher Release-Build über alle Hardware-Generationen hinweg (Rev 2.0 bis Rev 4.0) plattformübergreifend lauffähig.
---

### 8. Prädiktiver N-zu-N-Matrix-Optimierungsalgorithmus (Transitive Reduktion)
Zur Maximierung der transienten Erfassungsrate (860 Hz) arbeitet die Firmware nicht mit einem statischen, sequenziellen Durchlaufen aller Kreuzungspunkte, sondern implementiert eine dynamische, graphentheoretische Matrix-Reduktion zur Laufzeit:
* **Transitive Zustandsschlussfolgerung:** Der Algorithmus nutzt die mathematische Transitivität und den historisch ermittelten Systemzustand (Kopplung vs. Isolation), um redundante Messzyklen logisch zu eliminieren. Wird eine niederohmige FIE-Verbindung zwischen Knoten $A_1$ und $A_2$ detektiert sowie eine Isolation zwischen $B_1$ und $A_1$ nachgewiesen, schlussfolgert die State-Machine prädiktiv die Isolation zwischen $B_1$ und $A_2$. Ist zudem $B_1$ mit $B_2$ verbunden, wird die Isolation von $B_2$ zu $A_1$ und $A_2$ rein mathematisch verifiziert, ohne den physischen Messkanal abzufragen.
* **Präventive PGA-Vorselektion:** Basierend auf dem prädiktiv erwarteten Zustand steuert die Firmware das Register des internen Signalverstärkers (PGA) des ADS1115 vorausschauend an. Dies eliminiert Einschwingzeiten (Settling Times) und Sättigungs-Artefakte (Clipping) beim Kanalwechsel vollständig, wodurch die physikalische Zykluszeit auf das absolute Minimum komprimiert wird.

---

### 9. Hochauflösende Einzelpaar-Abtastung (Sub-Matrix-Modus)
Neben dem universellen N-zu-N-Gesamtscan unterstützt die Firmware die gezielte Reduktion der Schaltmatrix auf isolierte Einzelpaar-Prüfungen (z. B. dediziert A1-A2 oder B2-C1). Durch den vollständigen Entfall von Multiplexer-Umschaltzeiten (ADG707) und PGA-Registeranpassungen wird die transiente Abtastrate des 16-Bit-ADC (ADS1115) exakt auf den Zielkanal konzentriert. Dies maximiert die zeitliche Auflösung zur Detektion von Mikrosekunden-Kabelbrüchen unter mechanischer Last (Biegetest), während der Isolations- und Verbindungsstatus des Paares parallel autark abgeleitet wird.

---

### 10. Flankengesteuerte transiente Trigger-Engine & Pre-/Post-Speicherung
Für die detaillierte Analyse des mechanischen Schaltverhaltens von Waffen-Spitzenkontakten implementiert das System eine universelle Signal-Trigger-Engine im Echtzeit-Chart:
* **Flanken-Unabhängiger Trigger:** Die Firmware überwacht kontinuierlich den Gradienten der Widerstandsänderung. Bei Überschreiten der Schwellenwert-Flanke (unabhängig von der Richtung: Low-to-High / High-to-Low) friert das Grafik-Framework das Live-Chart instantan ein.
* **Einstellbares Pre-/Post-Trigger-Verhältnis (X-Prozent):** Der Stopp- und Visualisierungszeitpunkt des eingefrorenen Signalverlanfs ist über einen prozentualen Parameter ($X$) auf der Zeitachse frei konfigurierbar. Dies ermöglicht die exakte geometrische Positionierung des Schaltmomenten auf dem Display, wodurch transiente Prell-Effekte vor dem Stoß (Pre-Trigger) sowie Kontaktschwankungen nach dem mechanischen Anschlag (Post-Trigger) zur tiefgehenden Rüstmeister-Diagnose konserviert werden.

---

### 11. Asymmetrische UI/UX-Visualisierungs-Engine & Gestensteuerung
Zur intuitiven Fehlersuche im Feld implementiert die Core-UI auf Core 1 eine asymmetrische, farbcodierte Signal-Verstärkungs-Engine. Diese übersetzt hochfrequente transiente Messereignisse (860 Hz) in eine für das menschliche Auge optimierte visuelle Dynamik:
* **Asymmetrisches Fade- & Hold-Verfahren (Wackelkontakt-Hold):**
  * *Signal-Eintritt (Gut):* Ein stabiler niederohmiger Durchgang blendet zeitverzögert weich ein (Fade-in zu Grün). Ein transienter Kontaktausfall schaltet die Anzeige ohne Latenz augenblicklich hart um (Sofort-Rot).
  * *Fehler-Austritt (Schlecht):* Ein transienter Fehlerfall triggert sofort die Alarmfarbe Rot. Nach der Stabilisierung des Signals blendet die Warnfarbe verzögert aus (Fade-out). Dieser visuelle Hold-Effekt streckt Mikrosekunden-Einbrüche, wodurch sporadische Wackelkontakte ohne Trägheitsverlust für den Anwender sichtbar werden.
* **Topologie-identische Netz-Matrix & Spline-Visualisierung:** Die UI projiziert ein 1:1 geometrisches Abbild der physischen Buchsenanordnung auf das Display. Elektrische Zustände des prädiktiven N-zu-N-Scans werden in Echtzeit als dynamisches Vektor-Netzwerk dargestellt: Offene Kanäle verbleiben grau, gültige Verbindungen spanen grüne/gelbe Spline-Bögen, und Brückenfehler/Isolationsbrüche leuchten rot auf. Adernvertauschungen sind instantan geometrisch interpretierbar. Die Abklinggeschwindigkeit des Hold-Effekts ist über diskrete Stufen regulierbar (*Off / Fast / Medium / Slow*).
* **Gestenbasierte Landschafts-UI (Flow-Free Interaction):** Die Benutzeroberfläche verzichtet auf starre Menü-Strukturen und Double-Clicks. Die Navigation erfolgt fluid über Gesten (Wischen/Sliden in einer horizontalen UI-Landschaft), Touch-Bereiche und Long-Clicks. Dies gewährt maximale Bedienfreiheit und stellt sicher, dass Laien rein intuitiv über die Farbmetrik (Grün-Gelb-Rot) agieren können, während Rüstmeister parallel die vollständige physikalische Rohdaten-Transparenz erhalten.

---

### 12. Autonome Anschlusspaar-Suchlogik & Zustandshold-Ablauf
Zur Optimierung von Ein-Mann-Prüfungen an langen Hallenleitungen implementiert die Firmware eine zustandsgesteuerte Auto-Lock-Such-Engine:
* **Autonomer Suchmodus:** Solange kein gültiger Messkontakt vorliegt, befindet sich das System im kontinuierlichen Suchlauf. Die RGB-Signalisierung verbleibt hierbei in einem gedimmten, neutralen Weiß (visueller Grauwert).
* **Dynamische Messpaar-Verriegelung:** Bei Eintritt eines stabilen "OK"-Zustands arretiert die Firmware den Messfokus augenblicklich auf diesem dedizierten Paar, schaltet die LED in die aktive FIE-Farbmetrik und aktiviert die akustischen/haptischen Signalgeber. Die Matrix-Verriegelung bleibt so lange aktiv, bis das Signal für eine konfigurierbare, längere Zeitschwelle permanent im "NOK"-Bereich verbleibt. Erst nach Ablauf dieses Timeouts wechselt das Framework kreisfrei zurück in den globalen Suchmodus.

---

### 13. Multimodale Feedback-Kaskade (Buzzer / RGB-LDO / Haptik)
Jeder Messmodus verfügt über vollständig unabhängig konfigurierbare Signalisierungsparameter zur optimalen Anpassung an die Hallenakustik:
* **Akustik-Profil (Buzzer):** Softwareseitig modulierbar in den Stufen *Off / Short NOK / Long NOK / OK*.
* **Haptik-Profil (Vibrationsmotor):** Angesteuert über den dedizierten Treiber-IC NCP18255 zur direkten mechanischen Alarmierung bei transienten oder permanenten Fehlern (*Off / Short NOK / Long NOK*), um akustische Störungen in der Fechthalle zu umgehen.
* **Visuelles Umgebungs-Mapping (RGB-LED):** Gepuffert über eine 74LVC1617-Treiberstufe arbeitet die externe RGB-Status-LED synchron mit der asymmetrischen Fading- und Farb-Engine des Hauptdisplays. Die Grundhelligkeit ist zur Vermeidung von Blendeffekten bzw. zur Maximierung der Tageslichttauglichkeit vollständig per Software registergesteuert skalierbar.

---

### 14. FIE-Preset-Validierung & Sabotageschutz
Zur Einhaltung internationaler Wettkampfstandards implementiert die Firmware eine manipulationssichere Preset-Validierungs-Engine für die Materialkontrolle:
* **Permanentes Visuelles FIE-Feedback:** Das aktiv geladene FIE-Regelwerk mitsamt den exakten physikalischen Grenzwerten wird permanent transparent im UI dargestellt. Dies sichert eine unanfechtbare Nachvollziehbarkeit bei offiziellen Waffenkontrollen auf Turnieren.
* **Read-Only Firmware-Lock:** Die Kern-Grenzwerte sind im Firmware-Image als unveränderliche Read-Only-Konstanten fest verankert und hardwareseitig über die aktive Flash-Verschlüsselung (Flash Encryption) gegen jegliche lokale oder physische Manipulation geschützt. 
* **Zentralisiertes Webapp-Update:** Zukünftige offizielle Regeländerungen des Internationalen Fechtverbandes (FIE) können ohne Hardware-Modifikationen über die integrierte Webapp per passwortgeschütztem Over-the-Air-Update (OTA) eingespielt und im FRAM dynamisch aktualisiert werden.

---

### 15. Granulares JSON-Parameter-Framework & Asymmetrischer Krypto-Import
Für das effiziente Flottenmanagement implementiert die Firmware eine konfigurierbare Im-/Export-Schnittstelle auf JSON-Basis über die integrierte Webapp:
* **Selektiver Import via Kategorie-Filter:** Beim Einspielen der JSON-Konfigurationsdatei erlauben modulare Checkboxen das selektive Überschreiben spezifischer Datenklassen (z. B. Hardware-Kalibrierungsdaten vs. globale Systemeinstellungen).
* **Asymmetrische Credential-Sicherheit:** WiFi-Zugangsdaten (SSID/Passwort) werden beim Export aus dem Gerät kryptografisch verschlüsselt in die JSON-Struktur eingebunden. Der Import-Parser akzeptiert zur vereinfachten Erstprovisionierung neuer Geräte über den Administrations-PC jedoch auch unverschlüsselte Klartext-JSON-Profile.

---

### 16. Dual-Netzwerk-Topologie & Dynamische QR-Code-Kopplungs-Engine
Das Kommunikationsmodul unterstützt den parallelen oder exklusiven Betrieb als eigenständiger Access Point (AP-Mode) oder als Netzknoten im bestehenden Infrastruktur-Netzwerk (Client-Mode). Zur Eliminierung manueller Eingabefehler im Hallenbetrieb generiert das TFT-Grafikframework dynamische QR-Codes im Geräte-Display:
* **WiFi-AP-Connect:** Automatisierte Endgeräte-Kopplung an den verschlüsselten Access Point ohne manuelle Passworteingabe.
* **Webapp-Redirect:** Direkter Aufruf der REST/WebSocket-Schnittstelle via lokaler mDNS-Namensauflösung (`fencingtester.local`).
* **Public-Repo-Link:** Permanenter QR-Verweis auf das öffentliche GitHub-Repository zur abrufbaren Bereitstellung der Open-Hardware-Dokumentation im Feld.

---

### 17. Integrierter Recovery-Webserver & Autarker SD-Karten-OTA (Offline Rescue)
Zur Gewährleistung einer absoluten Ausfallsicherheit (Anti-Brick-Schutz) implementiert das System eine mehrstufige Notfall-Update-Kaskade:
* **Automatischer Notfall-OTA-Server:** Erkennt die Firmware beim Systemstart eine Beschädigung oder das vollständige Fehlen der webapp.gz in der LittleFS-Partition, startet die MCU autark einen minimalistischen, fest im Core-Image integrierten Recovery-Webserver. Dieser stellt via Direktaufruf eine native HTML-Schnittstelle bereit, über die die LittleFS-Dateistruktur direkt im Browser ohne externe Programmierwerkzeuge wiederhergestellt werden kann.
* **Hardwarenaher SD-OTA-Pfad:** Für den netzwerkunabhängigen Offline-Einsatz in isolierten Fechthallen verfügt das System über eine autarke SD-Karten-Update-Routine. Beim Booten detektiert die Firmware das Vorhandensein spezifischer Binärdateien auf dem SD-Speichermedium (J4-Schnittstelle) und triggert einen vollautomatischen lokalen Flash-Vorgang für die App- oder Filesystem-Bereiche.

---

### 18. Asymmetrisches UI-Design-Framework & Visuelle Skalierungslogik
Das visuelle Konzept folgt einer dualen Design-Philosophie, die strikte funktionale Konsistenz mit gerätespezifischer UX-Optimierung kombiniert:
* **Device-UI (Strikter Minimalismus):** Die hardwareintegrierte Benutzeroberfläche auf dem 2,8"-TFT-Display ist sachlich, unverschnörkelt und auf maximale Kontrastwerte optimiert. Jede visuelle Redundanz wird zugunsten einer fehlerfreien Ablesbarkeit im hektischen Hallenbetrieb unter schwierigen Lichtverhältnissen eliminiert.
* **Webapp-UI (Feingranulares Diagnose-Center):** Die Client-Side-Webapp spiegelt die Kern-Struktur und den Bedien-Flow der Device-UI 1:1 wider, nutzt jedoch die höhere Pixeldichte moderner Endgeräte-Displays aus. Durch feinere Vektorgrafiken, abgerundete UI-Elemente und strukturierte Hintergrund-Farbpaletten wird eine hochwertige High-End-Ästhetik erzielt.
* **Flächeneffizientes Informations-Mapping:** Der zusätzliche Anzeigeraum auf größeren Client-Bildschirmen wird explizit zur Skalierung hochauflösender Live-Widerstands-Charts und Trendkurven genutzt. Das Aufblähen von reinem Text wird systemisch verhindert. Der globale Informations- und Bedien-Flow ist streng linear von links nach rechts sowie von oben nach unten ausgerichtet und folgt der physischen Geometrie des äußeren Buchsenlayouts.
