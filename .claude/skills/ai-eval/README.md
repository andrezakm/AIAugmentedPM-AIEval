# AI Eval

Misst ein KI-Feature über einen Testsatz und mehrere Läufe gegen die Kriterien einer `eval.md` und sagt, ob die Schwellen fürs Vertrauen erreicht sind. Ein einzelner Blick auf eine Antwort reicht dafür nicht, weil KI-Features bei derselben Eingabe jedes Mal etwas anders antworten — das Ergebnis ist deshalb immer eine Quote „x von n", nicht ein einzelnes PASS oder FAIL.

## Aufruf

```
/ai-eval                                          Standard: alle Fälle × 3 Läufe
/ai-eval schnell                                  1 Lauf je Fall, nur Tendenz
/ai-eval nur prüfen output/2026-09-24/auto-reply/  vorhandene Läufe prüfen, keine neuen erzeugen
```

Feature-Skill, Testsatz-Ordner, Eval-Datei und Läufe je Fall haben Standardwerte (`auto-reply`, `testset/`, `feature/eval.md`, 3), lassen sich beim Aufruf aber alle ersetzen.

## Liest

- Die Eval-Datei — Kriterien, Prüfart, Anleitung und Schwellen kommen ausschließlich von dort.
- Die README des Testsatz-Ordners für die Soll-Tabelle.
- Die SKILL.md des Feature-Skills, um Läufe genau nach dessen Anleitung zu erzeugen.
- Im Modus „Nur prüfen" die vorhandenen Lauf-Dateien im benannten Ordner.
- Die jüngste frühere Messung (`ergebnis.md`), falls vorhanden, für den Vergleich.

## Schreibt

Jede Messung bekommt einen eigenen Ordner `output/YYYY-MM-DD/ai-eval/messung<M>/`, M zählt am selben Tag hoch. So kannst du vor und nach einer Änderung messen, ohne dass etwas überschrieben wird.

- `…/messung<M>/laeufe/<fall-id>-lauf<N>.md` — die erzeugten Läufe (entfällt im Modus „Nur prüfen").
- `…/messung<M>/ergebnis.md` — Gesamturteil, Ergebnistabelle, Fehlerliste, Muster, Prüfer-Stichprobe, nächster Schritt.

Verändert nie die Eval-Datei, den Testsatz oder den Feature-Skill.

## Grenzen

Der KI-Prüfer, der die Ja/Nein-Kriterien beantwortet, irrt auch mal — deshalb enthält das Ergebnis immer eine Stichprobe von sechs Urteilen zum Selbst-Nachprüfen, statt dass du ihm blind glaubst. Läufe, die alle in derselben Sitzung entstehen, sind nie ganz unabhängig voneinander; wo die Arbeitsumgebung isolierte Hilfsagenten anbietet, nutzt der Skill sie dafür, sonst steht ein entsprechender Hinweis im Ergebnis. Der Schnellmodus liefert bewusst nur eine Tendenz — für eine belastbare Aussage brauchst du die Standard-Messung.

## Auf ein eigenes Feature anwenden

Der Skill ist generisch: Er funktioniert für jedes KI-Feature, das einen eigenen Feature-Skill, einen eigenen Testsatz mit Soll-Tabelle in der README und eine eigene `eval.md` mit Kriterien, Prüfart und Schwellen hat. Baue dir diese drei Teile für dein Feature, und ruf `/ai-eval` mit den passenden Pfaden auf.
