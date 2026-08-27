# 🚀 SVA-Fencing-Tester – Release Checklist

Dieses Dokument dient als Leitfaden für die manuelle Release-Erstellung und das Deployment auf den öffentlichen `release`-Branch des Portals.

## 1. Vorbereitung & Code-Härtung (Privat)
- [ ] **Compiler-Schalter prüfen:** Sicherstellen, dass alle Test- und Debug-Bibliotheken (wie z. B. der alte FTP-Server oder serielle Ausgaben) für die Produktion per `#ifdef` deaktiviert sind.
- [ ] **Sicherheit aktivieren:** Firmware-Verschlüsselung aktivieren.
- [ ] **Bundle bauen:** Die lokale Webapp komprimieren (`webapp.gz`) und das LittleFS-Image inklusive des aktuellen `device_manual_lite.pdf` erstellen.
- [ ] **Kompilieren:** Den automatisierten Post-Build-Workflow ausführen, um das finale Kombi-Bundle (`.ota`) zu generieren.

## 2. Automatisierter Lizenz-Export (Yarn / Webapp)
Da die Webapp-Abhängigkeiten (Vue, Vite-Plugins, Bootstrap etc.) stark verschachtelt sind, wird der Export vor dem Release kurz automatisiert:
- [ ] Öffne das Terminal in deinem `webapp/`-Ordner.
- [ ] Führe folgenden Befehl aus, um alle aktiven Produktions-Lizenzen in eine Textdatei zu schreiben:
  ```bash
  npx license-checker-rseidelsohn --production --csv > webapp_lizenzen.txt
  ```

## 3. KI-gestützter Dokumentationsabgleich
- [ ] Kopiere den Inhalt der generierten `webapp_lizenzen.txt` und deine aktuelle `platformio.ini`.
- [ ] Übergib die Daten an die KI mit dem Befehl: 
  *"Hier sind meine aktuellen Webapp- und Firmware-Abhängigkeiten. Aktualisiere bitte die Tabelle in meiner docs/compliance/THIRD_PARTY_LICENSES.md und behalte das bestehende Format bei."*
- [ ] Kopiere die von der KI generierte Markdown-Tabelle und füge sie in deine `THIRD_PARTY_LICENSES.md` im Portal ein.
- [ ] Trage die neue Versionsnummer, den Build-Counter und das aktuelle Datum in die `bin/CHANGELOG.md` ein.

## 4. Manuelles Deployment (Public Portal)
- [ ] Wechsel in deinem Portal-Repository auf den Branch **`release`**.
- [ ] Kopiere das neue `.ota`-Bundle in den Ordner `bin/stable/`. 
- [ ] Lösche bei Bedarf uralte Builds, behalte aber die letzten *n* Versionen als schnelles Downgrade-Sicherheitsnetz im Ordner.
- [ ] Aktualisiere die `bin/version.json` mit der neuen Versionsnummer, dem exakten Dateinamen und dem korrekten GitHub-Raw-Link, der auf den `release`-Branch zeigt.
- [ ] Pushe die Änderungen auf den `release`-Branch.
- [ ] **Live-Test:** Verbinde ein Testgerät im Offline-WLAN, öffne die About-Page und prüfe, ob der Client-Browser den Versionsabgleich über die neue `version.json` fehlerfrei ausführt.
