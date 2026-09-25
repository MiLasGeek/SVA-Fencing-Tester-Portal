# SVA-Fencing-Tester-Portal 🤺

Willkommen im offiziellen Portal für den SVA-Fencing-Tester. Dieses Projekt wird als private Non-Profit-Initiative im Ehrenamt geführt, um Fechtvereine und Fechter mit einem zuverlässigen Werkzeug für die Wartung und Materialkontrolle zu unterstützen.

⚠️ **AKTUELLER STATUS: Entwicklungs- und Evaluierungsphase**
Dieses öffentliche Portal veröffentlicht OTA-Updates, Handbücher, technische Hinweise und signierte Prüfparameter. Firmware und Webapp bleiben Closed Source; Quellcode und editierbare Elektronik-Layouts werden hier nicht veröffentlicht. Nicht CE-gekennzeichnete Einheiten sind keine allgemeinen Produkte und nicht für Verbraucher freigegeben.

---

## 📂 Portal-Übersicht

* 🤖 **[Firmware & OTA-Updates](./bin)** – Enthält die `version.json` für den automatischen Versionsabgleich über den Client-Browser sowie die offiziellen Update-Bundles.
* 🤺 **[FIE-Presets](./fie)** – Lesbare, signierte FIE-Prüfparameter mit aktuellem Manifest, Archiv und direktem Download für die Geräte-Webapp.
* 📑 **[FIE Material Rules (December 2025)](https://static.fie.org/uploads/38/190667-book%20m%20ang.pdf)** – Externe Regelwerksreferenz des aktuellen FIE-Presets; Erreichbarkeit und Aktualität liegen bei der FIE.
* 📐 **[Hardware & 3D-Druck](./hardware)** – STL- und STEP-Dateien für das frei druckbare Basis-Gehäuse und alle gängigen Verschleißteile.
* 📖 **[Online-Handbuch & Dokumentation](./docs)** – Das vollständige Online-Handbuch, ausführliche Reparaturanleitungen und die Material-Stückliste (BOM).
* ⚙️ **[Technische Spezifikationen](./FEATURES.md)** – Das vollständige, detaillierte Leistungsspektrum und alle Geräteeigenschaften im Überblick.
* ⏳ **[Entwicklungsgeschichte & Veröffentlichungsnachweis](./DEVELOPMENT_HISTORY.md)** – Die Chronologie der Hardware-Revisionen (Rev 1.0 bis Rev 4.0).
* 💻 **[Software-Architektur & Algorithmen](./SOFTWARE_ARCHITECTURE.md)** – Dokumentation der deterministischen Echtzeit-Schnittstellen und Lock-Free-Puffer.
* ⚖️ **[Rechtliches & Impressum](./IMPRINT.md)** – Gesetzliche Anbieterkennzeichnung nach § 5 DDG sowie Haftungsausschlüsse für Software und Hardware.
* 🛡️ **[Datenschutzerklärung](./PRIVACY.md)** – Transparente Informationen zur DSGVO-konformen Datenverarbeitung innerhalb dieses Repositories.
* 📋 **[Compliance-Status (Entwurf)](./docs/compliance/README.md)** – Vorläufige CE-, CRA- und FIE-bezogene Unterlagen; keine finale Konformitätsdokumentation.
* 📜 **[Lizenzliste (Third-Party)](./docs/compliance/THIRD_PARTY_LICENSES.md)** – Übersicht über die verwendeten Open-Source-Bibliotheken und Drittlizenzen.
* 💬 **[Community & Support](../../discussions)** – Unser virtuelles Vereinsheim. Nutze den Tab "Discussions" für Fragen, Erfahrungsaustausch und Best Practices.
* 🐛 **[Fehler melden](../../issues)** – Nutze den Tab "Issues" für strukturierte Bug-Reports zur Firmware, Webapp oder Hardware.

---

## 🚀 Highlights & Kernfeatures (Sperrveröffentlichung)

Der SVA-Fencing-Tester (Revision 4.0) bricht mit traditionellen Messgeräte-Konzepten und bietet Profiliga-Diagnose im ultrakompakten Eurokarten-Format ($100 \times 60$ mm):

* **860-Hz-Echtzeit-Abtastung:** Lückenlose Erkennung transienter Wackelkontakte im Millisekundenbereich zur präzisen Materialkontrolle.
* **Prädiktiver Matrix-Scan:** Graphentheoretische Transitivitäts-Reduktion überspringt redundante Kreuzprüfungen und schont ADC-Einschwingzeiten.
* **Asymmetrisches Fading:** Visueller Hold-Effekt „streckt“ flüchtige Fehler für das menschliche Auge (Sofort-Rot bei Defekt, weiches, einstellbares Fade-out).
* **Topologie-identische UI:** 1:1 grafische Buchsen-Projektion auf dem Display mit dynamischen Spline-Bögen zur sofortigen Entlarvung von Adernvertauschungen.
* **Echtes Zero-Power-Standby:** Automatische PMIC-Abschaltung bei Inaktivität. Knopflose Wiederzuschaltung rein über transienten Kondensator-Einschaltstrom.
* **Masse-Anker-Stabilisierung:** Das mechanische Plangepresste Akku-Platinen-Sandwich nutzt die Batteriezelle als thermische Kapazität zur ADC-Rauschminimierung.
* **Bulletproof-Recovery-Kaskade:** Ein im Core integrierter Notfall-HTML-Server und ein lokaler microSD-OTA-Pfad sichern das System gegen Datenkorruption im Feld ab.

👉 **Das vollständige, detaillierte Leistungsspektrum findest du in der [FEATURES.md](./FEATURES.md).**

---

## 🔒 Technisches Konzept & Sicherheit (Closed-Source)

Dieses Projekt ist auf Code-Ebene Closed-Source. Es werden keine Quellcodes oder editierbaren Elektronik-Layouts veröffentlicht. Zum Schutz vor Manipulationen, zum Erhalt der Turniertransparenz bei Waffenkontrollen und zur Abwehr von Reverse Engineering greift folgende Architektur:

* **Hybrid-Updates über Client-Brücke:** Die Geräte arbeiten im Messbetrieb komplett offline und strahlen einen eigenen WLAN-Access-Point aus. Der Abgleich erfolgt über den Browser des Endgeräts (Smartphone/PC), welcher die Update-Datei von GitHub lädt und lokal per Webapp auf den Tester überträgt. Alternativ ist ein Offline-Update via microSD-Karte möglich.
* **All-in-One Custom Bundle:** Updates werden als verschlüsseltes Kombi-Paket bereitgestellt, welches die Firmware, das LittleFS-Dateisystem (inkl. der komprimierten Webapp `webapp.gz`) und ein schlankes Geräte-Handbuch enthält.
* **Automatischer Failsafe (Recovery Mode):** Die Integrität des Filesystems wird beim Systemstart geprüft. Fehlt die `webapp.gz` im Flash oder ist sie beschädigt, lädt der ESP32 automatisch einen autarken, minimalistischen Notfall-Webserver. Die Weboberfläche zur Rettung ist im Fehlerfall direkt über **`http://192.168.4.1`** erreichbar.
* **Hardware-Sicherheit:** Für die Releasekonfiguration ist die hardwareseitige AES-Verschlüsselung des ESP32-S3 (Flash Encryption) vorgesehen. Sie schützt die Vertraulichkeit der Flash-Inhalte, ersetzt jedoch keinen hardwareseitig erzwungenen Secure Boot oder physischen Manipulationsschutz. Die genaue Grenze ist in der Compliance-Dokumentation beschrieben.

---

## 📖 Dokumentation (Hybrid-Konzept)

Um den begrenzten Flash-Speicher des ESP32 zu schonen, teilen wir die Dokumentation auf:
1. **Offline-Handbuch (Lite):** Direkt im LittleFS des Geräts gespeichert. Es ist extrem speicheroptimiert, verzichtet auf große Bilder und beschreibt nur die physischen Tasten, LEDs und die Display-Menüs.
2. **Online-Handbuch (Full):** Hier im Repository unter `/docs/manuals`. Nur hier findest du die vollständige Anleitung inklusive hochauflösender Screenshots der Webapp und Erklärungen zu den Browser-Statistiken.

---

## 🛠️ Bezug von Hardware & Ersatzteilen (Non-Profit)

Es gibt keinen kommerziellen Webshop und keine allgemeine Geräteabgabe. Nicht CE-gekennzeichnete Einheiten werden ausschließlich als kundenspezifische Evaluierungskits an benannte Vereine mit sachkundiger Projektleitung abgegeben. Der Verein übernimmt die Endmontage einschließlich der eigenständigen Beschaffung und des Anschlusses eines geeigneten Akkus über den vorgesehenen Steckverbinder und verwendet das Kit ausschließlich in einer dokumentierten Forschungs- und Entwicklungsumgebung.

Das Projekt kann eine technische Akkuempfehlung bereitstellen. Der Akku wird nicht als Bestandteil des Evaluierungskits geliefert. Das vermeidet Gefahrgutversand und hilft, die Bereitstellungs- und Beschaffungskosten für Vereine niedrig zu halten. Er darf nur unbeschädigt, ohne Gewalt und gemäß der Kit-Anleitung angeschlossen werden. Die Endmontage durch den Verein ändert für sich allein weder die rechtliche Einordnung des Kits noch ersetzt sie die später erforderliche Konformitätsbewertung.

Die Übergabe der Evaluierungskits erfolgt in der Regel persönlich. Dadurch entfällt Versandverpackung; die Anforderungen für eine spätere allgemeine Marktbereitstellung bleiben davon unberührt. Bei Rückgaben ist Versand nicht ausgeschlossen, wird aber vorab abgestimmt.

Für die Rücknahme eines Evaluierungskits bitte vorab Kontakt aufnehmen. Der abgestimmte Rückgabeweg und eine Vorlage für die erforderlichen Angaben stehen unter [Rücknahme eines Evaluierungskits](./docs/return_request_template.md) bereit.

Private Fechter erhalten keine Einheit direkt, sondern nur über ihren Verein im Rahmen dieses Projektvorhabens. Eine andere Bereitstellung wird erst nach Abschluss der erforderlichen Laborprüfungen und der Konformitätsbewertung geprüft.

Ein Erwerb oder eine Kostenerstattung für ein Evaluierungskit wird ebenso individuell mit dem Verein abgestimmt; es gibt keinen offenen Shop und keine anonyme Direktbestellung. Dieser kleine, dokumentierte Rahmen erleichtert Übergabe, Rücknahme und Nachverfolgbarkeit. Er ersetzt jedoch nicht die rechtliche Prüfung, falls eine Überlassung als Marktbereitstellung einzuordnen ist.

Die technische Vorbereitung für die Laborprüfung ist abgeschlossen. Bei der kleinen Stückzahl liegen die Kosten voraussichtlich etwa beim Vier- bis Fünffachen der Kosten von zehn Geräten. Spenden und Fördermittel werden vorrangig für die EMV- und Funkprüfungen durch ein geeignetes Labor eingesetzt.

Eine CE-Erklärung wird erst nach abgeschlossener, dokumentierter Konformitätsbewertung unterzeichnet. Vor einer allgemeinen Marktbereitstellung werden außerdem die jeweils anwendbaren Pflichten für Elektrogeräte, Batterien und Verpackungen - einschließlich Kennzeichnung, Registrierung, Rücknahme und Entsorgung - geprüft und erfüllt.

Für eine einzelne B2B-Registrierung fallen nach der veröffentlichten Gebührenübersicht der stiftung ear derzeit beispielhaft rund 28,40 EUR netto einmalig sowie rund 131,20 EUR netto jährlich an, zuzüglich möglicher Entsorgungskosten. Bei einer Weitergabe von etwa zwanzig Geräten ist das ein erheblicher Fixkostenanteil. Die begrenzte Evaluierungsphase ermöglicht Vereinen daher einen preiswerten, dokumentierten Testeinsatz. Sie ist jedoch keine Zusage, dass keine Markt-, CE-, Entsorgungs- oder sonstigen Rechtsrisiken bestehen; jede Überlassung wird einzeln dokumentiert und rechtlich eingeordnet.

👉 **[Evaluierungskit-Anfrage für einen Verein erstellen](../../issues/new?template=anfrage_premium.md)**

---

## 🧾 Steuerlicher Hinweis & Abrechnung

* **Aktueller Rahmen:** Das Projekt wird von einer einzelnen Privatperson in der Freizeit betrieben; eine Gewinnabsicht besteht nicht. Solange keine andere Organisationsform oder gewerbliche Tätigkeit besteht, werden keine gesonderten Umsatzsteuerbeträge ausgewiesen.
* **Brutto-Selbstkosten:** Elektronische Bauteile werden über offizielle Distributoren wie Mouser und Aisler bezogen. Eine etwaige Weitergabe erfolgt zu den tatsächlich angefallenen Brutto-Materialkosten, ohne Gewinnaufschlag und nur im Rahmen der zeitlich möglichen Handbestückung.
* **Vereinsbuchhaltung:** Vereine erhalten einen privaten Kaufbeleg (Quittung über Aufwandsersatz/Materialkosten) für ihre Unterlagen. Kopien der originalen Distributor-Rechnungen können zur absoluten Transparenz beigelegt werden. 
* **Fertigung nach Verfügbarkeit:** Bestückung, Kalibrierung und Endtests erfolgen in der Freizeit und nur in kleinen Stückzahlen. Sollte sich die Organisations- oder Fertigungsform ändern, werden die rechtlichen, steuerlichen und vertraglichen Angaben vor einer Bereitstellung entsprechend überprüft und angepasst.

---

## 📄 Lizenz & Rechtliches

Die Firmware ist urheberrechtlich geschützt (Binary-only). Für die 3D-Druckdaten im Ordner `/hardware` gilt die **Creative Commons Namensnennung - Nicht-kommerziell - Weitergabe unter gleichen Bedingungen 4.0 International (CC BY-NC-SA 4.0)**. Eine gewerbliche Nutzung oder der kommerzielle Weiterverkauf der Gehäuseteile ist strikt untersagt. Siehe `LICENSE`-Datei für Details.

Informationen zum Anbieter sowie zum Datenschutz findest du in unserem separaten [Impressum](./IMPRINT.md) und der [Datenschutzerklärung](./PRIVACY.md).
