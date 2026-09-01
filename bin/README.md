# Firmware & OTA-Updates

Dieser Ordner enthält die OTA-Bundles für den SVA-Fencing-Tester sowie die Datei `version.json` für den Versionsabgleich in der Geräte-Webapp.

> **Entwicklungsstatus:** Solange das Portal als „Under Construction“ gekennzeichnet ist, sind bereitgestellte Dateien ausschließlich für Tests vorgesehen. Installiere nur ausdrücklich freigegebene Bundles auf einem Gerät.

## Update-Prinzip

Der Tester benötigt selbst keinen Internetzugang. Er stellt für die lokale Geräte-Webapp ein eigenes WLAN bereit. Ein Smartphone oder PC übernimmt die Internetverbindung, lädt das OTA-Bundle aus diesem Portal herunter und überträgt die Datei anschließend lokal über die Webapp an das Gerät.

## Update durchführen

1. Stelle sicher, dass der Akku ausreichend geladen ist und unterbrich die Stromversorgung während des Updates nicht.
2. Verbinde Smartphone oder PC mit dem WLAN des Testers und öffne die lokale Geräte-Webapp.
3. Öffne dort den Update-/Info-Bereich. Die Webapp prüft anhand von `version.json`, ob eine neuere Version verfügbar ist.
4. Öffne den angezeigten Portal-Link im Browser des Clients und lade das passende vollständige `.ota`-Bundle herunter.
5. Kehre zur Geräte-Webapp zurück, wähle die lokal gespeicherte `.ota`-Datei aus und starte die Übertragung auf den Tester.
6. Warte, bis die Webapp den erfolgreichen Abschluss meldet. Das Gerät startet danach gegebenenfalls neu.
7. Prüfe nach dem Neustart im Info-Bereich die installierte Version.

## Downgrade

Ältere, kompatible Bundles bleiben in [`stable/`](./stable/) verfügbar. Für ein Downgrade wird derselbe Ablauf verwendet. Installiere nur vollständige und für die jeweilige Hardware-Revision freigegebene OTA-Bundles.

## SD-Karten-Recovery

Falls ein reguläres Update nicht möglich ist oder die Webapp nicht geladen werden kann, nutze die [Anleitung zur SD-OTA-Recovery](../docs/manuals/sd_ota_recovery.md).

## Screenshots

Screenshots der einzelnen Webapp-Schritte werden nach Fertigstellung der finalen Update-Oberfläche ergänzt.
