# Special: Trauen wir uns den Autopiloten?

NordAntrieb GmbH (fiktiv) bekommt täglich rund 120 Kundenmails. Die Geschäftsführung möchte, dass ein KI-Feature Standardanfragen im Ziel automatisch beantwortet — ein Autopilot für den Kundenservice. Aktuell sind wir in der Testphase: Das Feature schreibt nur Antwortentwürfe, verschickt wird nichts. Die Frage dieses Specials ist, ob wir uns den Autopiloten schon trauen — und die Kriterien dafür stehen in `feature/eval.md`.

## Wie das Feature entscheidet

Jede Mail bekommt eine von zwei Entscheidungen: **beantworten** oder **eskalieren**. Grundlage dafür sind die acht Service-Regeln in `context/service-regeln.md` und die Firmendaten in `context/firma.md` — dieselbe Wissensbasis für jede einzelne Mail und jede Messung. Eskaliert wird immer dann, wenn eine Regel das verlangt, etwa bei angedrohten rechtlichen Schritten oder bei einer Kulanzentscheidung, die kein Feature allein treffen sollte.

## Warum ein eigenes Repo

Dein System-Repo aus Woche 5 und das Spec-und-Eval-Repo aus Woche 4 bleiben unberührt. Dieses Special braucht eigene Testmails, eigene Regeln und eine eigene Messung — deshalb ein eigenes, schlankes Repo, in dem du nichts kaputt machen kannst. Das Muster kennst du aus Woche 4: erst `spec.md`, dann `eval.md`. Neu ist, dass das Feature selbst eine KI ist und deshalb anders gemessen werden muss.

## Start

1. ZIP entpacken.
2. Den entpackten Ordner in VS Code öffnen.
3. GitHub Copilot Chat im Agent-Modus öffnen.
4. `/kurs` eintippen — der Kurs führt dich Schritt für Schritt durch das Special.

Kein Setup, keine Installation nötig. Das Feature verschickt zu keinem Zeitpunkt eine echte Mail.

## Ordnerübersicht

```
AIEval-CLAAS/
├── context/         Firmenprofil und die acht Service-Regeln
├── feature/         Spec und Eval des Auto-Reply-Features
├── testset/         acht Testmails mit Soll-Tabelle
├── doc/             Grafik zur Eval-Schleife und die Beispiel-Messung (drei echte Messungen)
├── output/          Lauf-Ergebnisse, nach Datum sortiert
└── .claude/skills/  die drei Befehle aus der Tabelle unten
```

## Die drei Befehle

| Befehl | Was er tut | Wann du ihn brauchst |
|---|---|---|
| `/kurs` | Führt Schritt für Schritt durch das Special | Als Einstieg, ganz am Anfang |
| `/auto-reply` | Beantwortet eine einzelne Mail oder empfiehlt eine Eskalation | Um an einer einzelnen Mail auszuprobieren, wie das Feature entscheidet |
| `/ai-eval` | Misst `/auto-reply` über den ganzen Testsatz gegen `feature/eval.md` | Für die eigentliche Messung — die Quote „x von n" |

## Was du hier lernst

Das Special vermittelt sechs Einsichten, die für jedes KI-Feature gelten, nicht nur für Mail-Antworten:

- KI-Antworten fallen bei derselben Mail jedes Mal etwas anders aus. Deshalb misst man nicht an einem einzigen Versuch, sondern über einen Testsatz und mehrere Durchläufe, als Quote.
- Bevor man zählt, schaut man sich die tatsächlichen Fehler an und sortiert, welche Art von Fehler das ist.
- Für jede Fehlerart nimmt man die einfachste Prüfung, die ausreicht — von reiner Textprüfung bis zur KI, die eine einzelne Ja/Nein-Frage beantwortet. Diesem KI-Prüfer schaut man per Stichprobe über die Schulter, denn auch er irrt.
- Nicht jeder Fehler wiegt gleich schwer. Eine übersehene Pflicht-Eskalation ist schlimmer als eine überflüssige, deshalb sind manche Schwellen strenger als andere.
- Was „gut genug" bedeutet, legt das Produkt fest, nicht ein Werkzeug — die Schwellen sind eine bewusste Entscheidung.
- Reicht eine Messung nicht, wird genau an der Stelle nachgeschärft, an der die Fehler entstehen, und danach erneut gemessen.

Diese sechs Punkte bilden zusammen die Denkweise hinter jeder Eval, egal für welches KI-Feature — das Beispiel NordAntrieb macht sie nur konkret erfahrbar.

Los geht's: Ordner öffnen, Copilot Chat im Agent-Modus starten, `/kurs` eintippen.
