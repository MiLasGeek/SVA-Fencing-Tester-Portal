# Firmware-Update und Wiederherstellung per SD-Karte (Offline-Update)

Falls kein WLAN verfügbar ist oder die Weboberfläche des Testers nicht mehr richtig lädt, können Sie das Gerät ganz einfach über eine SD-Karte aktualisieren oder reparieren.

---

## 💾 1. SD-Karte vorbereiten

Der Tester besitzt einen Standard-SD-Kartenschnittstelle. Sie können auch eine microSD-Karte mit einem passenden SD-Adapter nutzen.

**Voraussetzungen für die Karte:**
- **Größe:** Empfohlen werden Karten mit 4 bis 32 GB Speicherplatz.
- **Formatierung:** Die Karte muss im Format **FAT** oder **FAT32** formatiert sein. (Sehr große Karten ab 64 GB sind oft als 'exFAT' vorformatiert und müssen am PC zwingend auf FAT32 umgestellt werden).

**Schritte am PC:**
1. Laden Sie das aktuelle Update-Paket (die Datei mit der Endung `.ota`) aus dem `/bin`-Ordner unseres Portals herunter.
2. Kopieren Sie die Datei direkt auf die SD-Karte (nicht in einen Unterordner!).
3. Ändern Sie **nicht** den Namen der Datei. Der Tester ignoriert Dateien, die nicht exakt dem offiziellen Namensschema entsprechen.

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

---

## ⚡ 3. Problembehebung & Automatisches Update (Force Update)

### Das Gerät bietet das Update nicht mehr an?
Wenn Sie ein Update dreimal hintereinander ignorieren oder abbrechen (durch Tippen auf „Start device“), merkt sich der Tester dies auf der SD-Karte. Er wird Ihnen dieses spezifische Update danach nicht mehr automatisch vorschlagen, um Sie im Vereinsalltag nicht zu stören.

### Ein Update erzwingen („Force Update“):
Falls ein Update blockiert blockiert ist oder Sie den Bestätigungsdialog komplett überspringen möchten:
1. Erstellen Sie am PC eine leere Textdatei auf der SD-Karte und nennen Sie diese exakt: `SVA_OTA_FORCE.txt`.
2. Stecken Sie die Karte in den ausgeschalteten Tester und starten Sie ihn.
3. Das Gerät führt das Update nun **sofort und ohne jegliche Nachfrage** beim Starten aus. Die Datei wird dabei vom Tester automatisch gelöscht, um Endlos-Schleifen zu verhindern.
