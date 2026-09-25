# Messvalidierung und Genauigkeit

## Zweck

Der SVA-Fencing-Tester ist ein Materialtester für Fechtausrüstung. Er ist keine
Meldeanlage und ersetzt keine formale Materialabnahme durch die zuständigen
Stellen. Die technische Messvalidierung dokumentiert die Eignung der Messkette
für den vorgesehenen Prüfzweck; sie ist keine FIE-Zertifizierung.

## Messprinzip

Die Widerstandsmessung verwendet eine ratiometrische Vierleiter-
Vergleichsmessung nach dem Newton-Brücken-Prinzip. Der Messwert wird gegen einen
temperaturstabilen Präzisions-Referenzwiderstand bestimmt. Die tatsächliche
Referenz und die R0-Brückenkompensation werden bei der Geräteprovisionierung
ermittelt und für das jeweilige Gerät dokumentiert.

## Inhouse-Validierung

Für den validierten Hardware- und Firmwarestand ergab die Inhouse-Vermessung
ein Messrauschen von rund 3 mOhm. Damit besteht für die vorgesehenen
Widerstandsgrenzen ein ausreichender Abstand zu einer Auflösung von 0,1 Ohm.
Die eingesetzte Referenz hat eine Genauigkeit von 0,1 %.

Diese Angaben gelten nur für den dokumentierten Prüfaufbau und den getesteten
Gerätestand. Bei Änderungen an Hardware, Referenz, Messpfad oder Firmware wird
die Validierung wiederholt und ergänzt.

## Transparenter Prüfmodus

Das signierte FIE-Preset hält die verwendeten Grenzwerte, Version, Datum,
Quelle und Regelreferenzen lesbar fest. Bei aktivem Preset ist der FIE-Modus in
der Benutzeroberfläche sichtbar. Dadurch ist nachvollziehbar, mit welchen
Parametern geprüft wurde; daraus folgt keine formale FIE-Zertifizierung des
Testers oder eines einzelnen Prüfergebnisses.
