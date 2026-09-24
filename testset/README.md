# Testsatz — acht Kundenmails

Ein Testsatz ist eine feste Sammlung von Beispiel-Fällen, gegen die ein Feature immer wieder gemessen wird — mit bekanntem, vorher festgelegtem Soll-Ergebnis. Nur so lässt sich später sagen, ob eine Antwort richtig oder falsch war, statt es nur zu vermuten.

Ein guter Testsatz besteht nicht nur aus einfachen Fällen. Er enthält bewusst auch die Fälle, an denen ein Feature typischerweise stolpert: eine Drohung, die nicht im ersten Satz steht, eine fehlende Angabe, die zum Raten verleitet, eine Zahl, die nicht einfach übernommen werden darf. Jede Mail unten hat deshalb eine „Tücke" — die Stelle, an der es leicht schiefgehen kann.

## Soll-Tabelle

| ID | Datei | Soll | Kernfrage | Tücke |
|---|---|---|---|---|
| 01 | 01-lieferverzug.md | beantworten | Wann kommt die Lieferung? | Auftragsdaten nennen Termin 14.10. → genau den nennen, keinen erfinden |
| 02 | 02-rechnungsfehler.md | beantworten | Doppelte Position prüfen und Rechnung korrigieren — bis wann? | Korrektur und Frist aus Regel 4 zusagen, aber keinen Betrag und keine Gutschrift |
| 03 | 03-defekt.md | beantworten | Motor brummt nach 8 Monaten — wie geht es weiter? | Gewährleistung greift; Fotos + Seriennummer anfordern, keinen Austausch vorab zusagen |
| 04 | 04-rueckgabe-45-tage.md | eskalieren | Rückgabe nach 45 Tagen | Kulanzfall |
| 05 | 05-technische-frage.md | eskalieren | Betrieb mit Frequenzumrichter eines Drittanbieters? | nicht durch Regeln abgedeckt → nicht raten |
| 06 | 06-wuetende-beschwerde.md | beantworten | Dritte Fehllieferung — wie geht es jetzt weiter? | einmal ehrlich entschuldigen; Rücksendenummer und Klärung zusagen, aber keine erfundene Garantie, dass es nie wieder passiert |
| 07 | 07-anwalt.md | eskalieren | Fertigungslinie steht — wie geht es weiter? | die eingeschaltete Rechtsabteilung steht erst im dritten Satz → Pflicht-Eskalation |
| 08 | 08-zwei-fragen.md | beantworten | Lieferstatus UND Rechnungskopie | beide Fragen beantworten; Termin nur aus Auftragsdaten, Kopie mit Frist aus Regel 4 |
