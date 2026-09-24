# Special: Evals für AI-Features — Kontext-Regel

Dieses Repo ist ein eigenständiges Special zum Kurs AI-Augmented PM: Es prüft, ob ein KI-Feature verantwortbar automatisch auf Kundenmails von NordAntrieb GmbH (fiktiv) antworten darf. Es ist bewusst getrennt von den Repos der Wochen 4 und 5, damit dort nichts verändert wird. Wer hier arbeitet, bewegt sich also in einer Testumgebung mit fiktiven Firmen, fiktiven Personen und fiktiven Vorgängen.

## Wo was liegt

- `context/firma.md` und `context/service-regeln.md` — die Wissensbasis. Vor jeder Antwort und vor jeder Messung lesen.
- `feature/spec.md` — was die automatische Antwort tun soll.
- `feature/eval.md` — woran sich entscheidet, ob wir dem Feature trauen, mit Schwellen.
- `testset/` — die acht Testmails und die Soll-Tabelle, gegen die gemessen wird.

Ein einzelner Antwortlauf und eine vollständige Messung über den Testsatz sind zwei verschiedene Schritte: Der eine probiert an einer Mail aus, der andere sagt, ob die Schwellen insgesamt erreicht sind. Beide bauen auf derselben Wissensbasis auf.

## Ausgabe

Jeder Lauf landet unter `output/YYYY-MM-DD/<skill>/` — ein Tagesordner, darunter ein Unterordner pro Befehl. Frühere Läufe bleiben unverändert stehen, damit sich Stände vergleichen lassen.

## Feste Regeln

Das Feature verschickt nie eine Mail und behauptet nie, etwas verschickt zu haben — es ist Testphase, es entstehen ausschließlich Entwürfe. `feature/eval.md` und die Dateien unter `testset/` bleiben während eines Messlaufs unverändert, sie sind die Referenz, an der gemessen wird. Nur wenn ein Mensch ausdrücklich entscheidet, dass sich die Kriterien oder der Testsatz ändern sollen, werden sie angepasst — nie beiläufig im Rahmen eines Laufs.

Alle Ausgaben auf Deutsch, mit korrekten Umlauten. Eine Antwort erfindet nie einen Termin, einen Betrag oder eine Zusage, die nicht in den Service-Regeln oder den Auftragsdaten der jeweiligen Mail steht — im Zweifel wird eskaliert statt geraten.
