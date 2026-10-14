# Hardware-Übersicht

Dieser Bereich enthält ausgewählte, nicht-editierbare Visualisierungen der Hardware-Revisionen des SVA-Fencing-Testers. Schaltpläne, PCB-Quelldaten und Fertigungsdaten sind nicht Bestandteil dieses öffentlichen Portals.

## PCB-Renderings

- [Revision 2](./pcb/rev2/)
- [Revision 4](./pcb/rev4/)
- [Westen-Prüfstift-Adapterplatine](./pcb/vest-probe-adapter/)

## 3D-Modelle für Gehäuse-CAD

Die KiCad-exportierten STEP-Modelle enthalten die bestückten PCB-Baugruppen und dienen als räumliche Grundlage für Gehäusekonstruktionen:

- [Revision 2](./models/sva-fencing-tester-rev2.step)
- [Revision 4](./models/sva-fencing-tester-rev4.step)
- [Westen-Prüfstift-Adapterplatine](./models/pcb-adapter-v1.step)

Die Abbildungen dienen ausschließlich zur Identifikation und Dokumentation der jeweiligen Hardware-Revision. Sie sind keine Fertigungsunterlagen.

## Planungsausblick: Revision 5

Revision 5 befindet sich in der Konzept- und Planungsphase. Geplant ist, den Funktionsumfang von Revision 4 beizubehalten und die Bedienoberfläche auf ein 3,5-Zoll-Display mit 480 × 320 Pixeln zu erweitern. Das Display soll über ein 8-Bit-i8080-Interface mit 2D-DMA-Unterstützung angebunden werden, um die höhere Auflösung mit einer höheren Bildwiederholrate darzustellen.

Vorgesehen ist ein ESP32-S3 sowie ein größerer Akku. Das größere Display, die erweiterte Systemplattform und die Energieversorgung erhöhen voraussichtlich die Materialkosten. Das bisherige Aluminium-Strangpressgehäuse ist für das 3,5-Zoll-Display nicht ohne Weiteres geeignet; die Gehäusekonstruktion wird deshalb im Zuge von Rev 5 neu geplant. Es gibt derzeit keine Rev-5-Renderings, CAD- oder Fertigungsdaten. Alle Angaben sind Planungsstand und können sich ändern.

## Westen-Prüfstift-Adapterplatine

Die separate Adapterplatine verbindet einen Westen-Prüfstift mit dem vorgesehenen Vest-Test-Anschluss. Die Ansichten dokumentieren die bestückte Baugruppe; sie enthalten keine editierbaren Leiterplatten-, Schaltplan- oder Fertigungsdaten.
