# SVA-Fencing-Tester-Portal 🤺

Willkommen im offiziellen Portal für den SVA-Fencing-Tester. Dieses Projekt wird als private Non-Profit-Initiative im Ehrenamt geführt, um Fechtvereine und Fechter mit einem zuverlässigen Werkzeug für die Wartung und Materialkontrolle zu unterstützen.

⚠️ **AKTUELLER STATUS: UNDER CONSTRUCTION (Entwicklungsphase)**
Dieses Repository befindet sich aktuell im Aufbau und dient dem Live-Test der Device-Webapp-Verlinkung. Die hier hinterlegten Firmware-Dateien, Schaltpläne und Dokumente sind reine Testversionen und noch nicht für den produktiven Einsatz freigegeben. Anfragen für Hardware-Komponenten sind in dieser Phase noch deaktiviert.

---

## 📂 Portal-Übersicht

* 🤖 **[Firmware & OTA-Updates](./bin)** – Enthält die `version.json` für den automatischen Versionsabgleich über den Client-Browser sowie die offiziellen Update-Bundles.
* 📐 **[Hardware & 3D-Druck](./hardware)** – STL- und STEP-Dateien für das frei druckbare Basis-Gehäuse und alle gängigen Verschleißteile.
* 📜 **[Dokumentation & Compliance](./docs)** – Das vollständige Online-Handbuch, Reparaturanleitungen, die Material-Stückliste (BOM) sowie Nachweise zu CE, CRA und FIE-Konformität.
* 💬 **[Community & Support](../../discussions)** – Unser virtuelles Vereinsheim. Nutze den Tab "Discussions" für Fragen, Erfahrungsaustausch und Best Practices.
* 🐛 **[Fehler melden](../../issues)** – Nutze den Tab "Issues" für strukturierte Bug-Reports zur Firmware, Webapp oder Hardware.

---

## 🔒 Technisches Konzept & Sicherheit (Closed-Source)

Dieses Projekt ist auf Code-Ebene Closed-Source. Es werden keine Quellcodes oder elektronischen Schaltpläne (Gerber-Dateien) veröffentlicht. Zum Schutz vor Manipulationen und unbefugtem Auslesen (Reverse Engineering) greift folgende Architektur:

* **Hybrid-Updates über Client-Brücke:** Die Geräte arbeiten komplett offline und strahlen einen eigenen WLAN-Access-Point aus. Der Abgleich erfolgt über den Browser des Endgeräts (Smartphone/PC), welcher die Update-Datei von GitHub lädt und lokal per Webapp auf den Tester überträgt. Alternativ ist ein Update via microSD-Karte möglich.
* **All-in-One Custom Bundle:** Updates werden als verschlüsseltes Kombi-Paket bereitgestellt, welches die Firmware, das LittleFS-Dateisystem (inkl. der komprimierten Webapp `webapp.gz`) und ein schlankes Geräte-Handbuch enthält.
* **Automatischer Failsafe (Recovery):** Die Integrität der Webapp wird beim Systemstart geprüft. Fehlt die `webapp.gz` im Flash oder ist beschädigt, lädt der ESP32 automatisch einen autarken Recovery-Server. Die Weboberfläche zur Rettung ist direkt über **`http://192.168.4`** erreichbar.
* **Hardware-Sicherheit:** Vor dem ersten offiziellen Release wird die ESP32-Hardwareverschlüsselung (Flash Encryption) aktiviert. Das Auslesen des Flash-Speichers über physische Pins ist dadurch zwecklos.

---

## 📖 Dokumentation (Hybrid-Konzept)

Um den begrenzten Flash-Speicher des ESP32 zu schonen, teilen wir die Dokumentation auf:
1. **Offline-Handbuch (Lite):** Direkt im LittleFS des Geräts gespeichert. Es ist extrem speicheroptimiert, verzichtet auf große Bilder und beschreibt nur die physischen Tasten, LEDs und die Display-Menüs.
2. **Online-Handbuch (Full):** Hier im Repository unter `/docs/manuals`. Nur hier findest du die vollständige Anleitung inklusive hochauflösender Screenshots der Webapp und Erklärungen zu den Browser-Statistiken.

---

## 🛠️ Bezug von Hardware & Ersatzteilen (Non-Profit)

Es gibt keinen kommerziellen Webshop. Alle Bereitstellungen erfolgen privat auf Non-Profit-Basis und direkt auf Anfrage über unsere integrierten GitHub-Formulare, sobald die Testphase beendet ist.

* **DIY-Variante (Open Source):** Das Basis-Gehäuse sowie alle Verschleißteile (Tasterkappen, Gehäuseclips etc.) können über die Dateien im Hardware-Ordner für den Eigenbedarf frei gedruckt werden.
* **Komplettgeräte & vorbestückte Platinen:** Für Vereine ohne eigenen 3D-Drucker oder Lötausrüstung fertigen wir optimierte Premium-Gehäuse, vorbestückte Custom-PCBs (inkl. Akku mit JST-Stecker und Powerbank-Platine) sowie Komplettsysteme auf Anfrage.

👉 **[Hier eine Anfrage für Hardware oder Ersatzteile erstellen](../../issues/new?template=anfrage_premium.md)** *(In der Testphase inaktiv)*

---

## 🧾 Steuerlicher Hinweis & Abrechnung

* **Keine gewerbliche Rechnung:** Da es sich um eine rein private, ehrenamtliche Unterstützung von Fechter zu Fechter handelt, wird keine eigene Umsatzsteuer ausgewiesen und keine gewerbliche Rechnung ausgestellt.
* **Brutto-Selbstkosten:** Alle elektronischen Bauteile werden ordnungsgemäß versteuert über offizielle Distributoren (mouser, Aisler) eingekauft. Die Weitergabe an Vereine erfolgt zu diesen tatsächlichen Brutto-Materialpreisen ohne jegliche Gewinnabsicht.
* **Vereinsbuchhaltung:** Vereine erhalten einen privaten Kaufbeleg (Quittung über Aufwandsersatz/Materialkosten) für deren Unterlagen. Kopien der originalen Distributor-Rechnungen können zur Transparenz beigelegt werden. 
* **Fertigung nach Verfügbarkeit:** Da die Bestückung und Tests vollständig in der Freizeit stattfinden, erfolgt die Fertigung ausschließlich nach zeitlicher Verfügbarkeit. Bitte plant entsprechende Wartezeiten ein.

---

## 📄 Lizenz
Die Firmware ist urheberrechtlich geschützt (Binary-only). Für die 3D-Druckdaten im Ordner `/hardware` gilt die **Creative Commons Namensnennung - Nicht-kommerziell - Weitergabe unter gleichen Bedingungen 4.0 International (CC BY-NC-SA 4.0)**. Eine gewerbliche Nutzung oder der kommerzielle Weiterverkauf der Gehäuseteile ist strikt untersagt. Siehe `LICENSE`-Datei für Details.
