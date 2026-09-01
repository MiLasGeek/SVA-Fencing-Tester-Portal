# Firmware-Update und Wiederherstellung per SD-Karte (Offline-Update)

Falls kein WLAN verfügbar ist oder die Weboberfläche des Testers nicht mehr richtig lädt, können Sie das Gerät ganz einfach über eine SD-Karte aktualisieren oder reparieren.

---

## 💾 1. SD-Karte vorbereiten

Der Tester besitzt einen Standard-SD-Kartensteckplatz. Sie können auch eine microSD-Karte mit einem rein passiven microSD-auf-SD-Adapter nutzen.

**Voraussetzungen für die Karte:**
- **Größe:** Empfohlen werden SD-/SDHC-Karten mit 4 bis 32 GB Speicherplatz.
- **Formatierung:** Die Karte muss im Format **FAT** oder **FAT32** formatiert sein. SDXC-Karten ab 64 GB sind meist mit `exFAT` formatiert und müssen vor der Nutzung auf FAT32 formatiert werden.

**Schritte am PC:**
1. Laden Sie das aktuelle Update-Paket (die Datei mit der Endung `.ota`) aus dem `/bin`-Ordner unseres Portals herunter.
2. Kopieren Sie die Datei direkt auf die SD-Karte (nicht in einen Unterordner!).
3. Ändern Sie **nicht** den Namen der Datei. Der Tester ignoriert Dateien mit ungültigem Namen, Header oder Größe.
4. Werfen Sie die Karte am PC sicher aus und stecken Sie sie vor dem Einschalten in den Tester.

> Liegen mehrere gültige OTA-Bundles im Stammverzeichnis, verwendet der Tester automatisch die höchste gefundene Version.

---

## 🔌 2. Update oder Reparatur durchführen

1. Schalten Sie den SVA-Fencing-Tester komplett **aus**.
2. Schieben Sie die vorbereitete SD-Karte in den Kartenslot des Testers.
3. Schalten Sie das Gerät **ein**.

### Was passiert auf dem Bildschirm?
Der Tester erkennt die Update-Datei und zeigt Ihnen für **10 Sekunden** einen Dialog auf dem Display an:

*   **Knopf „Start device“ (Normaler Start):** Wenn Sie hier tippen (oder die 10 Sekunden ablaufen lassen), startet das Gerät ganz normal, ohne etwas zu verändern.
*   **Zweiter Knopf („Upgrade“ / „Downgrade“ / „Recover“):** Tippen Sie hier, um die Aktion zu starten. Das Gerät erkennt automatisch, was zu tun ist:
    *   **Upgrade:** Es wird eine neuere Version aufgespielt.
    *   **Downgrade:** Es wird eine ältere Version aufgespielt.
    *   **Recover (Wiederherstellung):** Das System repariert sich selbst, falls wichtige Systemdateien oder die Weboberfläche beschädigt wurden.

⚠️ **WICHTIG:** Schalten Sie den Tester während des Update-Vorgangs niemals aus und entnehmen Sie die SD-Karte nicht, bis der Vorgang vollständig abgeschlossen ist!

Nach einem erfolgreichen Update startet das Gerät automatisch neu. Prüfen Sie anschließend im Info-Bereich die installierte Version.

---

## ⚡ 3. Problembehebung & Automatisches Update (Force Update)

### Das Gerät bietet das Update nicht mehr an?
Wenn Sie dasselbe Downgrade dreimal hintereinander durch Tippen auf „Start device“ oder durch Ablauf der 10 Sekunden überspringen, merkt sich der Tester dies auf der SD-Karte. Er wird dieses spezifische Bundle bei einer funktionsfähigen Webapp danach nicht mehr automatisch vorschlagen, damit der Vereinsalltag nicht durch wiederholte Hinweise gestört wird. Neuere Updates und Wiederherstellungen bleiben davon unberührt.

### Ein übersprungenes Downgrade wieder anbieten

Soll derselbe Downgrade wieder angeboten werden, schalten Sie den Tester aus, stecken die SD-Karte in einen PC und löschen im Stammverzeichnis ausschließlich die Datei `SVA_OTA_DISMISSED.txt`. Danach wird das Bundle beim nächsten Start wieder geprüft und angeboten.

> Diese Datei hebt nur die Dreifach-Sperre für übersprungene Downgrades auf. Sie startet kein Update und ersetzt nicht die Force-Datei. Ein bereits erfolgreich installiertes Bundle derselben Version kann weiterhin als bereits angewendet erkannt werden.

### Ein Update erzwingen („Force Update“):
Falls ein zulässiges Bundle erneut angewendet werden soll oder Sie den Bestätigungsdialog bewusst überspringen möchten:
1. Erstellen Sie am PC eine leere Textdatei auf der SD-Karte und nennen Sie diese exakt: `SVA_OTA_FORCE.txt`.
2. Stecken Sie die Karte in den ausgeschalteten Tester und starten Sie ihn.
3. Das Gerät führt das Update nun **sofort und ohne Nachfrage** beim Starten aus. Die Datei wird vor dem Updateversuch automatisch gelöscht. Für einen weiteren Force-Update-Versuch muss sie deshalb erneut angelegt werden.

Nutzen Sie `SVA_OTA_FORCE.txt` nur, wenn Sie genau wissen, welches Bundle sich auf der Karte befindet.
