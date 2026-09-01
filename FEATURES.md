# ⚡ Feature-Übersicht & Leistungsspektrum (Features & Specifications)

Der SVA-Fencing-Tester (Revision 4.0) ist ein ultrakompaktes, proprietäres Prüf- und Diagnosegerät für den Fechtsport. Die Firmware wird als Closed-Source-Binary bereitgestellt. Es kombiniert multimodale Echtzeit-Messtechnik mit intuitiver visueller Analyse und industrieller Ausfallsicherheit.

---

### 🔬 Messtechnik & Diagnose-Performance
* **860-Hz-Echtzeit-Sampling:** Ultra-hochfrequente Abtastung zur lückenlosen Erfassung transienter Wackelkontakte im Millisekundenbereich.
* **Dualer Messbereich (0,01 Ω bis 10 MΩ):** Hochpräzise Durchgangsprüfung ($< 10\,\Omega$ mit $0,01\,\Omega$ Auflösung) und hochohmige Isolationsüberwachung ($> 500\,\text{k}\Omega$) in einer einzigen Matrix.
* **Prädiktiver Matrix-Scan:** Graphentheoretische Transitivitäts-Reduktion zur drastischen Verkürzung der physischen Abfragezyklen der N-zu-N-Kreuzmatrix.
* **Sub-Matrix-Paarprüfung:** Gezielte Fokussierung der vollen ADC-Abtastrate auf ein einzelnes Adernpaar (z. B. A1-A2) zur dedizierten, hochauflösenden Fehlersuche.
* **Flankengesteuerte Trigger-Engine:** Speicheroszilloskop-Modus für Waffenspitzen (Öffner/Schließer) mit frei einstellbarem Pre- und Post-Trigger-Verhältnis ($X\,\%$) im Live-Chart.

---

### 🎨 Visuelle UI/UX & Bedienkomfort
* **Asymmetrisches Fading (Fehler-Verstärker):** Visuelle Hold-Streckung für Mikrosekunden-Fehler (sofortiges hartes Rot bei Defekt, weiches Fade-out nach Stabilisierung). Abklingzeit einstellbar (*Off / Fast / Medium / Slow*).
* **Topologie-identische Netz-Matrix:** 1:1 grafisches Abbild der physischen Buchsen auf dem Display. Kurzschlüsse und Adernvertauschungen werden als dynamische, farbcodierte Spline-Bögen direkt visualisiert.
* **Duale Skalierungs-Balken mit Peak-Hold:** Lineare Anzeige für Durchgang ($0-10\,\Omega$) und logarithmische, invers gefüllte Darstellung für Isolation ($500\,\text{k}\Omega-10\,\text{M}\Omega$) inklusive dauerhafter Schlechtwert-Markierung.
* **Flow-Free Gestensteuerung:** Intuitive Navigation in einer horizontalen UI-Landschaft via Wischen, Touch-Bereichen und Long-Clicks – optimiert für die Bedienung in der Halle.
* **FIE-Preset-Transparenz:** Permanente Visualisierung des aktiven Regelwerks im Display. Manipulationssicherer Sabotageschutz (Read-Only Firmware-Lock) auf Geräteebene.

---

### 🔌 Konnektivität & Flottenmanagement
* **Dual-WiFi-Modus (AP & Client):** Autarker Access Point für die Planche und Client-Mode zur komfortablen Einbindung ins Vereins-Netzwerk.
* **Dynamische QR-Code-Kopplung:** Knopflose Verbindung per Scan direkt im Display (WiFi-AP-Connect, Webapp-Redirect via mDNS, Direktlink zum Public GitHub-Repo).
* **JSON-Parameter-Klonen:** Vollständiger Export und Import aller Einstellungen zur sekundenschnellen Flottenprovisionierung. Granulare Auswahl der Datenklassen via Checkbox-Filter beim Import.
* **Asymmetrische Credential-Sicherheit:** Verschlüsselte WiFi-Zugangsdaten im JSON-Export bei gleichzeitiger Akzeptanz von Klartext-Profilen zur Erstkonfiguration am PC.
* **Zentralisiertes Webapp-Update:** Passwortgeschütztes Over-the-Air-Update (OTA) für künftige FIE-Regelanpassungen direkt über den Browser.

---

### 🛡️ Hardware-Härtung, Power & Ausfallsicherheit
* **Echtes Zero-Power-Standby (0 Watt):** Automatische Abschaltung der 5V-Hauptschiene bei Inaktivität via Last-Unterschreitung des PMICs. Knopfloses Aufwecken rein über transienten Kondensator-Einschaltstrom (Inrush Current) durch kurzes Betätigen des Netzschalters ($< 1\,\text{s}$).
* **Peripherie-Trennung via LDO-Enable:** Vollständige physische und spannungslose Trennung der analogen +4V-Messstrecke im Deep-Sleep zur Eliminierung von Kriechströmen und Verhinderung von Akku-Restentladung.
* **Wartungsfreie RTC-Zeithaltung:** Direktes Akku-Backup der Echtzeituhr im Nanoampere-Bereich (weit unter der LiPo-Selbstentladung). Keine separate Knopfzelle nötig.
* **Temperaturkompensierte Batterie-Überwachung:** Silizium-Diode mit Gegenlauf-Kennlinie gleicht thermische Effekte des LiPos im Feld autark aus. Inhärente Fail-Safe-Sicherung gegen Überspannung bei Pfad-Unterbrechung.
* **Thermodynamisches Sandwich-Design:** Plangepresster 1800-mAh-LiPo-Akku wirkt als massive thermische Kapazität, dämpft Umgebungsschwankungen auf der Platine und eliminiert ADC-Rauschen. Schonende Laderate ($400\,\text{mA} \approx 0,22\,\text{C}$) schützt das TFT-Display vor Hitze.
* **Bulletproof Recovery-Kaskade:** Bei korruptem Filesystem startet die Firmware automatisch einen integrierten HTML-Notfall-Webserver zur Systemrettung im Browser. Zusätzlicher, autarker Offline-Flashpfad über integrierten SD-Kartenslot ($J4$).
* **Designed for Manufacturing & Repair:** Kompakte Eurokarten-Geometrie ($100 \times 60\,\text{mm}$) passend für Standard-Aluminium-Strangpressprofile ohne Platinenüberhang (keine Kollision mit tiefen 4-mm Frontbuchsen). Striktes Einseiten-SMD-Layout (Bottom) für kostengünstigen Single-Reflow-Nutzen inklusive Stencil-Schablone, vollständig handlötbar (Zero BGA/QFN) und mechanisch entkoppelte Anschlussbuchsen.
