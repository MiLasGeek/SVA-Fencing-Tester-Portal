# ⏳ Entwicklungsgeschichte & Prioritätsnachweis (Development History)

Dieses Dokument dient dem lückenlosen Nachweis der eigenständigen, ehrenamtlichen Entwicklung des SVA-Fencing-Testers. Die dauerhafte, kryptografisch zeitgestempelte Veröffentlichung dieser Chronologie und der zugrundeliegenden Schaltungsprinzipien auf GitHub begründet den rechtlichen Status des **„Standes der Technik“** (Defensive Publishing). Eine exklusive Patentierung oder Monopolisierung der hier offengelegten Verfahren durch Dritte ist damit weltweit ausgeschlossen.

---

### 📅 Technische Evolution & Prototypen-Chronologie

#### 1. Die Geburtsstunde: Breadboard-Validierung (August 2024)
* **Status:** Funktionaler Labor-Proof-of-Concept im fliegenden Aufbau.
* **Ergebnis:** Validierung des volldifferentiellen, ratiometrischen Messprinzips im ungeschirmten Zustand. Am Stichtag (**01.08.2024**) wurde die messtechnische Stabilität des mathematischen Verfahrens mit einer Auflösung von 0,01 Ω im niederohmigen Spektrum erfolgreich nachgewiesen (dokumentierter Laborwert: 2,20 Ω). Der Messbereich erstreckte sich in dieser Phase von 0,01 Ω bis 2 MΩ.
* **Prioritätsnachweis:** Einbindung des verifizierten Labor-Chat-Protokolls vom 01.08.2024 inklusive digitaler EXIF-Metadaten im Projekt-Asset-Ordner (`docs/assets/hist_01_breadboard_2024.png`).

#### 2. Revision 2.0: Die Prototypen-Serie & Labor-Debugging
* **Status:** Erste physische Leiterplatten-Fertigung (100x80 mm, 2-Layer).
* **Fertigung:** Bestellung von 15 nackten Platinen. Aufgrund der naturgemäß hohen Fehlerrate erster Platinen-Revisionen wurde aus Gründen der Kosteneffizienz vollständig auf eine Lötpasten-Schablone (Stencil) verzichtet; 10 Systeme wurden rein manuell an der Laborbank bestückt.
* **Iterative Härtung der 10 Prototypen:**
  * *Board 1 (Das Ur-Entwicklungsboard):* Ausgestattet mit dedizierten Pegelwandlern (Level Shifter) für den I²C-Bus und intensivem Hardware-Rework (Fädeldrähte/Zusatzkomponenten).
  * *Board 2:* Radikaler Rückbau zur Schaltungsvereinfachung. Die Level-Shifter-ICs wurden als überflüssig identifiziert, entfernt und durch direkte Drahtbrücken ersetzt. Der Bus-Betrieb erfolgte fortan rein über die Open-Drain-Physik.
  * *Board 3:* Erste Optimierung einer analogen P-Kanal-FET-Spannungsfolgerschaltung. Diese wurde notwendig, da ein genutzter Digital-Potentiometer-IC herstellerseitig undokumentierte (nur indirekt im Fließtext des Datenblatts erwähnte) Schottky-Schutzdioden an den Wiper-Kontakten besaß, die massive Kriechströme verursachten. Mittels manuellem Rework (Cut Traces, Zusatz-FETs und Widerstände) wurde eine Gate-Steuerspannung von -1,7 V bis -5 V zur analogen Pegelstellung des Buzzers realisiert.
  * *Board 4 bis 10:* Finalisierter, stabiler Umbau basierend auf Board 3, der als exakte Kopiervorlage für die restliche Kleinserie diente. Während des Betriebs zeigte sich ein hochgradig temporärer, vom LiPo-Akkuladestand abhängiger Bootloop-Fehler. Transiente Spannungsflanken triggerten die Reset-Leitung der DS3231-RTC, was durch ein physisches Auftrennen der Leiterbahn (Leiterbahntrennung) dauerhaft behoben wurde.

#### 3. Revision 3.0: Routing-Machbarkeit & Formfaktor-Schrumpf
* **Status:** Layout-Bereinigung ohne physische Zwischenfertigung.
* **Ergebnis:** Erfolgreiche Übernahme aller v2-Erkenntnisse (vollständige Entfernung der Level Shifter, I²C-Kopplung direkt über 3,3V-Pull-ups). Schrumpfung des Formfaktors um 25 % auf das kompakte Endmaß von **100x60 mm**, um die mechanische Kompatibilität zu standardisierten Aluminium-Strangpressprofilen zu sichern. Das Routing fokussierte primär die geometrische Machbarkeit im engen Raum, was verkoppelte Masseflächen mit langen Signal-Furchen und isolierten Masse-Inseln hinterließ.

#### 4. Revision 4.0: Serienreife, EMV-Härtung & Low-Cost-Optimierung
* **Status:** Finales Serien-Layout (100x60 mm, ready for Stencil-Sammelbestellung).
* **Layout-Härtung:** Vollständige Bereinigung der v3-Routing-Schwächen. Wiederherstellung absolut durchgehender, ununterbrochener Masseflächen (Ground Planes) auf beiden Lagen zur effektiven Abschirmung von Antenneneffekten und Störeinkopplungen der langen Fechtprüfleitungen im Hallenbetrieb.
* **Akustik-Revolution:** Kompletter Rauswurf der komplexen v2-Poti-Regelschleife. Ersetzung durch eine unhörbare, softwareseitig steuerbare 100-kHz-Dual-PWM-N-FET-Kaskade (Volume-PWM via passiven RC-Tiefpass an N-FET im Linearmodus kaskadiert mit digitalem Tone-N-FET gegen GND) für eine absolut verschleißfreie und knackfreie Lautstärkeregelung ohne mechanische Gehäusedurchbrüche.
* **Eingangsschutz:** Integration von ultra-kapazitätsarmen (Low-C) Fail-Short-ESD-Schutzdioden-Arrays aus dem High-Speed-USB-Design zum Schutz der analogen 16-Bit-Messkanäle (ADS1115/ADG707), ohne die kritische 860-Hz-Abtastung zu dämpfen. Bei massiver Überlastung verschmelzen die Dioden kontrolliert zu einem permanenten Kurzschluss gegen Masse, schützen den Systemkern vor Zerstörung und bleiben dank des handlötoptimierten Designs (Zero BGA/QFN) dezentral austauschbar.

---

### 🔬 Kernmerkmale des geschützten Messprinzips

1. **Ratiometrisches Hochgeschwindigkeits-Verfahren:** Nutzung des internen programmierbaren Signalverstärkers (PGA) des 16-Bit-ADC zur dynamischen, softwareseitigen Bereichsumschaltung unter Verzicht auf kaskadierte Hardware-Referenzwiderstände. Dies sichert eine konstante Fullrange-Abtastung von bis zu **860 Hz** zur Wackelkontakt-Analyse (0,01 Ω Auflösung im Bereich < 10 Ω, Trendanalyse bis 10 MΩ).
2. **Unified-Firmware mit I²C-Autoscan:** Die Firmware führt beim Systemstart einen automatischen Peripheriescan durch. Sie erkennt modular das Fehlen von RTC (Fallstand: keine Uhrzeit), sekundärem ADC (Wechsel von Parallelsampling auf sequenzielles Time-Multiplexing) oder FRAM (Wechsel auf EEPROM/Flash) und sichert die vollständige Abwärtskompatibilität über alle Hardware-Generationen hinweg mit einem einzigen Release-Build.
3. **Zero-Power-Standby (< 1€ PMIC-BOM):** Gezielte Ausnutzung der geräteinternen Mindestlast-Abschaltung extrem günstiger, massenproduzierter All-in-One-Power-Management-ICs. Bei softwareseitig ausgelöstem Deep-Sleep sinkt die Last unter die Erkennungsschwelle, wodurch der PMIC die 5V-Hauptschiene physisch kappt. Das System verbleibt bei absolut **0 Watt Standby**, bis ein manueller Spannungs-Reset über den Netzschalter erfolgt.
