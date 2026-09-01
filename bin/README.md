# Firmware & OTA-Updates

Dieser Ordner enthält die OTA-Bundles für den SVA-Fencing-Tester sowie die Datei `version.json` für den Versionsabgleich in der Geräte-Webapp.

> **Entwicklungsstatus:** Solange das Portal als „Under Construction“ gekennzeichnet ist, sind bereitgestellte Dateien ausschließlich für Tests vorgesehen. Installiere nur ausdrücklich freigegebene Bundles auf einem Gerät.

## Update-Prinzip

Der Tester benötigt selbst keinen Internetzugang. Er stellt für die lokale Geräte-Webapp ein eigenes WLAN bereit. Ein Smartphone oder PC übernimmt die Internetverbindung, lädt das OTA-Bundle aus diesem Portal herunter und überträgt die Datei anschließend lokal über die Webapp an das Gerät.

## Update durchführen

1. Stelle sicher, dass der Akku ausreichend geladen ist und unterbrich die Stromversorgung während des Updates nicht.
2. Verbinde Smartphone oder PC mit dem WLAN des Testers und öffne die lokale Geräte-Webapp.
3. Öffne dort den Update-Bereich und wähle **Neueste Version laden**. Die Webapp lädt das vollständige Bundle über die Internetverbindung des Clients, verifiziert dessen SHA-256-Prüfsumme und bietet es anschließend zur lokalen Übertragung an.
4. Prüfe Version und Hinweise und starte die Übertragung auf den Tester. Das Gerät selbst benötigt dabei keinen Internetzugang.
5. Alternativ kann eine bereits gespeicherte `.ota`-Datei über die Dateiauswahl hochgeladen werden. Das ist auch der Weg für ein gezieltes Downgrade.
6. Warte, bis die Webapp den erfolgreichen Abschluss meldet. Das Gerät startet danach gegebenenfalls neu.
7. Prüfe nach dem Neustart im Info-Bereich die installierte Version.

## Downgrade

Ältere, kompatible Bundles bleiben in [`stable/`](./stable/) verfügbar. Für ein Downgrade wird derselbe Ablauf verwendet. Installiere nur vollständige, ausdrücklich freigegebene OTA-Bundles.

## `version.json` für die direkte Aktualisierung

Die Geräte-Webapp liest die aktuelle Freigabe aus `version.json`. Damit der Button **Neueste Version laden** aktiv genutzt werden kann, benötigt `latest_release` zusätzlich zu den beschreibenden Feldern diese Werte:

```json
{
  "latest_release": {
    "version": "1.2.3+45",
    "filename": "SVA-Fencing-Tester_1.2.3+45.ota",
    "download_url": "https://raw.githubusercontent.com/MiLasGeek/SVA-Fencing-Tester-Portal/main/bin/stable/SVA-Fencing-Tester_1.2.3%2B45.ota",
    "sha256": "64-stellige-kleingeschriebene-sha256-pruefsumme"
  }
}
```

Die Datei wird erst nach vollständigem Download und erfolgreicher Prüfsummenprüfung als Update-Datei angeboten. Solange noch kein freigegebenes Bundle existiert, bleiben `filename` und `sha256` bewusst weg; die Webapp bietet dann keine Direktinstallation an.

## SD-Karten-Recovery

Falls ein reguläres Update nicht möglich ist oder die Webapp nicht geladen werden kann, nutze die [Anleitung zur SD-OTA-Recovery](../docs/manuals/sd_ota_recovery.md).

## Screenshots

Screenshots der einzelnen Webapp-Schritte werden nach Fertigstellung der finalen Update-Oberfläche ergänzt.
