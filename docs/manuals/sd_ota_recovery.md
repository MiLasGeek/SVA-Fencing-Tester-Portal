# SD-OTA und Recovery

Mit einer SD-Karte kann ein vollstaendiges OTA-Bundle ohne Netzwerk eingespielt werden. Der Ablauf ist fuer Upgrade, Downgrade und die Wiederherstellung einer unvollstaendigen Webapp geeignet.

## SD-Karte vorbereiten

Der eingebaute Sockel ist fuer Karten im Standard-SD-Format. microSD-Karten funktionieren mit einem rein passiven microSD-auf-SD-Adapter ebenso.

Unterstuetzt werden Karten mit FAT oder FAT32:

- SD und SDHC, empfohlen mit 4 bis 32 GB Kapazitaet.
- SDXC-Karten ab 64 GB nur, wenn sie vorher als FAT32 formatiert wurden. Das ab Werk uebliche exFAT wird nicht unterstuetzt.
- Fuer Service und OTA reicht eine kleine SDHC-Karte vollstaendig aus; das Bundle belegt derzeit nur wenige Megabyte.

1. Die Karte mit FAT/FAT32 formatieren.
2. Das erzeugte kombinierte `.ota`-Bundle in das Stammverzeichnis der Karte kopieren.
3. Die Karte vor dem Einschalten einstecken.

Der Dateiname muss dem Build-Schema entsprechen, zum Beispiel:

```text
SVA-Fencing-Tester.develop_0.7.0+70_2026-08-17_08_16.ota
```

Dateien mit ungueltigem Namen, Header oder Groesse werden ignoriert. Bei mehreren gueltigen Bundles waehlt das Geraet die hoechste gefundene Version.

## Startdialog

Nach dem SD-Mount erscheint bei einer passenden OTA-Datei fuer zehn Sekunden ein Touch-Dialog:

- `Start device` setzt den normalen Start fort. Dieser Button ist der Default und besitzt die ablaufende Leiste am unteren Rand.
- Der zweite Button fuehrt je nach Zustand `Upgrade`, `Downgrade` oder `Recover` aus.

Die Aktion folgt der Version des SD-Bundles: neuer bedeutet `Upgrade`, aelter bedeutet `Downgrade` und die gleiche Version bedeutet `Recover`. Ein Bundle gleicher Version wird nur dann ohne Dialog uebersprungen, wenn `SVA_OTA_APPLIED.txt` zu diesem Geraet passt und die Webapp gesund ist. Fehlt `webapp_dist/index.html.gz` oder `webapp_dist/js/app.js.gz`, wird ein gleiches Bundle daher ebenfalls als `Recover` angeboten. Fehlende Logos oder andere optionale Assets loesen keinen Recovery-Modus aus.

Die Dialogsprache folgt der gespeicherten Geraetesprache. Bei neuen oder nicht lesbaren Parametern ist Englisch der Default.

## Wiederholte Downgrade-Hinweise

Ein normales Ueberspringen eines Downgrades durch Touch auf `Start device` oder durch Timeout wird auf der SD-Karte gezaehlt. Die Datei `/SVA_OTA_DISMISSED.txt` enthaelt:

```text
MAC|appCRC|webCRC|count
```

Nach drei Ueberspringungen wird dieses Bundle auf diesem Geraet bei gesunder Webapp nicht mehr angeboten. Ein anderes Bundle startet wegen anderer CRCs wieder bei null. Upgrade und Recovery ignorieren diesen Zaehler.

`/SVA_OTA_APPLIED.txt` wird nur nach einer erfolgreichen OTA geschrieben und enthaelt die MAC-Adresse. Bei gleicher Karte und gleichem Geraet verhindert er erneute Angebote fuer gleich alte oder aeltere Bundles.

## Einmaliges Force-Update

Die leere Datei `/SVA_OTA_FORCE.txt` im SD-Stammverzeichnis erzwingt die Anwendung des ausgewaehlten gueltigen Bundles ohne Dialog. Sie umgeht Versionsvergleich, Erfolgsmarker und Dismiss-Zaehler.

Die Force-Datei wird vor dem Updateversuch geloescht. Sie ist daher einmalig und kann keine Boot-Schleife verursachen. Bei einem Fehler startet das Geraet anschliessend normal; fuer einen weiteren Force-Versuch muss die Datei erneut auf die Karte gelegt werden.

## Sicherheit und Speicherbedarf

Firmware- und Web-Payload werden vor der Aktivierung per CRC geprueft. Beim SD-OTA wird das Webarchiv direkt von der Karte in den LittleFS-Staging-Bereich entpackt; es wird keine zweite Archivkopie in LittleFS angelegt. Die vorhandene `webapp_dist` wird vor dem Leeren nach PSRAM gesichert und bei einem Fehler nach Moeglichkeit wiederhergestellt.

Fuer OTA und Rollback ist eine Variante mit mindestens 8 MB PSRAM erforderlich (`N8R8` oder `N16R8`).

Waerend der Aktualisierung das Geraet nicht ausschalten und die SD-Karte nicht entfernen.
