# SVA-Fencing-Tester-Portal 🤺

Willkommen im offiziellen Portal für den SVA-Fencing-Tester. Dieses Projekt wird als private Non-Profit-Initiative im Ehrenamt geführt, um Fechtvereine und Fechter mit einem zuverlässigen Werkzeug für die Wartung und Materialkontrolle zu unterstützen.

⚠️ **AKTUELLER STATUS: UNDER CONSTRUCTION (Entwicklungsphase)**
Dieses Repository befindet sich aktuell im Aufbau und dient dem Live-Test der Device-Webapp-Verlinkung. Die hier hinterlegten Firmware-Dateien, Schaltpläne und Dokumente sind reine Testversionen und noch nicht für den produktiven Einsatz freigegeben. Anfragen für Hardware-Komponenten sind in dieser Phase noch deaktiviert.

---

## 📂 Portal-Übersicht

* 🤖 **[Firmware & OTA-Updates](./bin)** – Enthält die `version.json` für den automatischen Versionsabgleich über den Client-Browser sowie die offiziellen Update-Bundles.
* 📐 **[Hardware & 3D-Druck](./hardware)** – STL- und STEP-Dateien für das frei druckbare Basis-Gehäuse und alle gängigen Verschleißteile.
* 📖 **[Online-Handbuch & Dokumentation](./docs)** – Das vollständige Online-Handbuch, ausführliche Reparaturanleitungen und die Material-Stückliste (BOM).
* ⚙️ **[Technische Spezifikationen](./FEATURES.md)** – Das vollständige, detaillierte Leistungsspektrum und alle Geräteeigenschaften im Überblick.
* ⏳ **[Entwicklungsgeschichte & Prioritätsnachweis](./DEVELOPMENT_HISTORY.md)** – Die lückenlose Chronologie der Hardware-Revisionen (Rev 1.0 bis Rev 4.0).
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
* **Hardware-Sicherheit:** Vor dem ersten offiziellen Release wird die hardwareseitige AES-Verschlüsselung des ESP32-S3 (Flash Encryption) dauerhaft aktiviert. Das physische Auslesen des Flash-Speichers über die GPIO-Pins ist dadurch kryptografisch wirkungslos.

---

## 📖 Dokumentation (Hybrid-Konzept)

Um den begrenzten Flash-Speicher des ESP32 zu schonen, teilen wir die Dokumentation auf:
1. **Offline-Handbuch (Lite):** Direkt im LittleFS des Geräts gespeichert. Es ist extrem speicheroptimiert, verzichtet auf große Bilder und beschreibt nur die physischen Tasten, LEDs und die Display-Menüs.
2. **Online-Handbuch (Full):** Hier im Repository unter `/docs/manuals`. Nur hier findest du die vollständige Anleitung inklusive hochauflösender Screenshots der Webapp und Erklärungen zu den Browser-Statistiken.

---

## 🛠️ Bezug von Hardware & Ersatzteilen (Non-Profit)

Es gibt keinen kommerziellen Weboss-Shop. Alle Bereitstellungen erfolgen privat auf Non-Profit-Basis und direkt auf Anfrage über unsere integrierten GitHub-Formulare, sobald die Testphase beendet ist.

* **DIY-Variante (Freier Nachdruck):** Das Basis-Gehäuse sowie alle Verschleißteile (Tasterkappen, Gehäuseclips etc.) können über die Designdateien im Hardware-Ordner für den Eigenbedarf frei gedruckt werden.
* **Komplettgeräte & vorbestückte Platinen:** Für Vereine ohne eigenen 3D-Drucker oder Lötausrüstung fertigen wir optimierte Premium-Gehäuse, im einseitigen Reflow-Verfahren vorbestückte Custom-PCBs (inkl. Akku mit JST-Stecker, separater Powerbank-Platine und passgenauer Stencil-Schablone) sowie Komplettsysteme auf Anfrage im On-Demand-Sammelverfahren.

👉 **[Hier eine Anfrage für Hardware oder Ersatzteile erstellen](../../issues/new?template=anfrage_premium.md)** *(In der Testphase inaktiv)*

---

## 🧾 Steuerlicher Hinweis & Abrechnung

* **Keine gewerbliche Rechnung:** Da es sich um eine rein private, ehrenamtliche Unterstützung von Fechter zu Fechter handelt, wird keine Umsatzsteuer ausgewiesen und keine gewerbliche Rechnung ausgestellt.
* **Brutto-Selbstkosten:** Alle elektronischen Bauteile werden ordnungsgemäß versteuert über offizielle Distributoren (Mouser, Aisler) eingekauft. Die Weitergabe an Vereine erfolgt zu diesen tatsächlichen Brutto-Materialpreisen ohne jegliche Gewinnabsicht, meist gesammelt in On-Demand-Chargen für je 10 Systeme zur Versandkosten-Optimierung.
* **Vereinsbuchhaltung:** Vereine erhalten einen privaten Kaufbeleg (Quittung über Aufwandsersatz/Materialkosten) für ihre Unterlagen. Kopien der originalen Distributor-Rechnungen können zur absoluten Transparenz beigelegt werden. 
* **Fertigung nach Verfügbarkeit:** Da die Bestückung, die Factory-$R_0$-Kalibrierung und die Endtests vollständig in der Freizeit stattfinden, erfolgt die Fertigung ausschließlich nach zeitlicher Verfügbarkeit. Bitte plant entsprechende Wartezeiten ein.

---

## 📄 Lizenz & Rechtliches

Die Firmware ist urheberrechtlich geschützt (Binary-only). Für die 3D-Druckdaten im Ordner `/hardware` gilt die **Creative Commons Namensnennung - Nicht-kommerziell - Weitergabe unter gleichen Bedingungen 4.0 International (CC BY-NC-SA 4.0)**. Eine gewerbliche Nutzung oder der kommerzielle Weiterverkauf der Gehäuseteile ist strikt untersagt. Siehe `LICENSE`-Datei für Details.

Informationen zum Anbieter sowie zum Datenschutz findest du in unserem separaten [Impressum](./IMPRINT.md) und der [Datenschutzerklärung](./PRIVACY.md).
