---
name: ai-eval
description: Misst ein KI-Feature über einen Testsatz und mehrere unabhängige Läufe gegen die Kriterien einer eval.md und sagt, ob die Schwellen fürs Vertrauen erreicht sind — nutze diesen Skill, um ein Feature wie den Antwort-Entwurf-Skill systematisch zu prüfen, statt einzelne Ausgaben nur stichprobenartig zu betrachten. Funktioniert für jedes Feature mit eigenem Feature-Skill, Testsatz und eval.md.
allowed-tools: Read, Write, Glob
---

# Skill: AI Eval

KI-Features antworten bei derselben Eingabe nicht jedes Mal gleich. Ein einzelner Blick auf eine Ausgabe sagt deshalb wenig — gemessen wird über einen Testsatz und mehrere Läufe, das Ergebnis ist eine Quote „x von n", nicht ein PASS oder FAIL. Dieser Skill führt diese Messung durch und hält sich dabei an die Kriterien, Prüfarten und Schwellen, die in der jeweiligen eval.md stehen — nichts davon ist in diesem Skill fest verdrahtet, damit er für jede eval.md eines beliebigen Features gleich funktioniert.

## Schritt 0: Run-Verzeichnis bestimmen

Berechne das heutige Datum im Format YYYY-MM-DD. Jede Messung bekommt einen eigenen Ordner `output/YYYY-MM-DD/ai-eval/messung<M>/` — ersetze YYYY-MM-DD durch das tatsächliche Datum; M ist die nächste freie Nummer an diesem Tag (per Glob prüfen, welche `messung*`-Ordner es schon gibt; keiner vorhanden: `messung1`). So bleibt jede Messung erhalten, auch wenn am selben Tag vor und nach einer Änderung gemessen wird. Erstelle das Verzeichnis implizit durch den ersten Write-Aufruf. Im Folgenden steht `<messordner>` für genau diesen Ordner.

## Einstellungen und Standardwerte

Alles hier ist beim Aufruf überschreibbar. Ohne Angabe gilt der Standard:

- **Feature-Skill:** `auto-reply` (dessen SKILL.md liegt unter `.claude/skills/auto-reply/SKILL.md`)
- **Testsatz-Ordner:** `testset/` (Soll-Tabelle in dessen README)
- **Eval-Datei:** `feature/eval.md`
- **Läufe je Fall:** 3 (Standard-Messung) bzw. 1 (Schnellmodus)

## Modus bestimmen

Erkenne den Modus aus dem Aufruf, oder frage kurz nach, wenn er nicht eindeutig ist:

- **(a) Messung** — Standardmodus. Alle Fälle des Testsatzes × 3 Läufe werden erzeugt und vollständig geprüft.
- **(b) Schnellmodus** — 1 Lauf pro Fall. Liefert nur eine Tendenz, keine Freigabe — das wird im Kopf, im Gesamturteil und in der Ergebnistabelle durchgängig so vermerkt.
- **(c) Nur prüfen** — beim Aufruf wird ein Ordner mit bereits vorhandenen Lauf-Dateien benannt (z. B. ein Ordner, den der Feature-Skill selbst schon gefüllt hat). Es werden keine neuen Läufe erzeugt, nur die vorhandenen geprüft.

## Vorgehen

Die Reihenfolge ist Absicht: Erst entstehen die Läufe, dann werden Eval-Datei und Soll-Tabelle gelesen. Wer die Kriterien und die richtigen Antworten schon kennt, schreibt unwillkürlich auf die Prüfung hin — die Messung wäre dann geschönt.

1. **Läufe beschaffen:**
   - Im Modus „Nur prüfen": Ermittle per Glob die vorhandenen Lauf-Dateien im benannten Ordner und ordne sie anhand des Dateinamens (`<fall-id>-lauf<N>.md`) den Fällen zu. Es werden keine neuen Läufe erzeugt.
   - In den Modi Messung/Schnellmodus: Ermittle per Glob die Fälle im Testsatz-Ordner (die Mail-Dateien, ohne dessen README zu öffnen) und erzeuge für jeden Fall die festgelegte Anzahl Läufe. Lies dazu die SKILL.md des Feature-Skills vollständig und folge ihrer Anleitung genau — mit einer Abweichung: Die Lauf-Datei landet nicht im Ausgabeordner des Feature-Skills, sondern unter `<messordner>/laeufe/<fall-id>-lauf<N>.md`. Jeder Lauf entsteht ohne Blick auf frühere Läufe desselben oder eines anderen Falls.
   - Bietet die Arbeitsumgebung ein Mittel an, um Teilaufgaben an isolierte Hilfsagenten zu delegieren, die keinen Einblick in die übrige Sitzung haben: Erzeuge jeden Lauf-Durchgang (also z. B. „Lauf 1 über alle Fälle", „Lauf 2 über alle Fälle") in einem eigenen Hilfsagenten. Das macht die Läufe voneinander unabhängiger, weil ein Hilfsagent die Ausgaben der anderen Durchgänge nicht sieht.
   - Steht ein solches Mittel nicht zur Verfügung: Erzeuge die Läufe der Reihe nach, Fall für Fall, Lauf für Lauf. Vermerke am Ende im Abschnitt „Hinweis zur Unabhängigkeit", dass die Unabhängigkeit dadurch eingeschränkt ist.
2. Lies jetzt erst die Eval-Datei vollständig: Kriterien, Prüfart je Kriterium, die Anleitung „Wie prüfen" und die Schwellen fürs Vertrauen. Diese Angaben bestimmen alles Weitere — nichts davon wird angenommen oder aus Erfahrung ergänzt.
3. Lies die README des Testsatz-Ordners für die Soll-Tabelle (welcher Fall soll beantwortet, welcher eskaliert werden; Kernfrage; ggf. weitere Soll-Werte).
4. **Prüfen — für jeden Lauf und jedes Kriterium aus der eval.md, entsprechend seiner Prüfart:**
   - **Textprüfung:** Wende die unter „Wie prüfen" beschriebenen Regeln wörtlich an. Wo eine Zahl eine Rolle spielt (z. B. eine Wortgrenze), zähle tatsächlich nach, schätze nicht.
   - **Soll-Vergleich:** Vergleiche die tatsächliche Entscheidung bzw. Ausgabe des Laufs mit dem Soll-Wert aus der Soll-Tabelle des Testsatz-Ordners.
   - **KI-Prüfer:** Beantworte die in der eval.md formulierte Ja/Nein-Frage streng, mit genau einem Satz Begründung. Sieh dir dafür nur an, was die Frage selbst braucht — die Frage, den Fall und die Antwort, bei Fragen zu Zusagen zusätzlich die Regeln bzw. die Auftragsdaten. Sieh dir nie die anderen Läufe an, damit das Urteil nicht von ihnen gefärbt wird.
   - Ein Kriterium, das sich nur auf beantwortete Fälle bezieht, gilt bei einem Lauf mit Entscheidung ESKALIEREN als „entfällt" und zählt nicht ins n. Das n in der Ergebnistabelle muss immer nachvollziehbar sein — trage dazu direkt daneben, aus wie vielen tatsächlich beantworteten Läufen es sich zusammensetzt.
5. Bilde je Kriterium das Ergebnis als „x von n", stelle die Schwelle aus der eval.md daneben und markiere, ob sie erreicht ist.
6. Formuliere das Gesamturteil in einem Satz, der mit genau einer dieser beiden Formen beginnt: „Schwellen nicht erreicht — der Autopilot bleibt aus." (mindestens eine Schwelle verfehlt) oder „Alle Schwellen erreicht — jetzt entscheidet der Mensch: erst Prüfer-Abgleich, dann Freigabe oder nicht." Danach höchstens ein Halbsatz zum Wichtigsten, etwa welche Schwelle knapp ist. Eine Schwelle, die genau auf der Grenze liegt, gilt als erreicht; die Entscheidungsregel wird nie eigenmächtig verschärft, und die Freigabe trifft dieser Skill nie selbst. Im Schnellmodus immer mit dem Zusatz „Tendenz, keine Freigabe".
7. Stelle die Fehlerliste zusammen: pro Fehler Fall/Mail, Lauf, Begründung, Link auf die Lauf-Datei — gruppiert nach Kriterium.
8. Beschreibe in 2–4 Sätzen, was die Fehler je Kriterium gemeinsam haben — ein Muster, kein bloßes Aufzählen.
9. Wähle eine Prüfer-Stichprobe von 6 KI-Prüfer-Urteilen, jeweils mit Link auf die Lauf-Datei zum Selbst-Nachprüfen. Mindestens 3 davon sind Urteile „bestanden" aus dem strengsten KI-Prüfer-Kriterium (dem mit der Schwelle „keine Ausnahme"), verteilt über verschiedene Fälle — die gefährlichsten Prüferfehler sind die übersehenen, und die zeigen sich nur unter den bestandenen Urteilen. Den Rest mischst du über die übrigen KI-Prüfer-Kriterien, gern mit Grenzfällen. Schreib dazu, dass der Mensch die Stichprobe auch selbst wählen kann und sollte, wenn ihm beim Lesen etwas aufgefallen ist.
10. Formuliere als nächsten Schritt genau eine gezielte Änderung — an der Spezifikation oder den Regeln des Features, nie an der Eval selbst. Ergänze den Hinweis, danach neu zu messen und mit diesem Ergebnis zu vergleichen.
11. **Vergleich mit einem früheren Ergebnis:** Prüfe per Glob, ob unter `output/` bereits frühere Messungen liegen (`*/ai-eval/messung*/ergebnis.md`). Wenn ja, ist die jüngste davon „vorher" (spätestes Datum, dort die höchste Messnummer vor der aktuellen). Ergänze in der Ergebnistabelle eine Spalte „vorher" und ordne die Veränderung je Kriterium in einem Satz ein — mit dem Hinweis, dass bei 3 Läufen kleine Schwankungen normal sind und nicht sofort als Trend zu lesen sind.
12. Schreibe `<messordner>/ergebnis.md` in der festen Reihenfolge unten. Alle Links darin sind relativ zur ergebnis.md (z. B. `laeufe/01-lieferverzug-lauf3.md`, im Modus „Nur prüfen" entsprechend relativ zum geprüften Ordner), nie absolute Pfade.

Verändere nie die Eval-Datei, den Testsatz oder den Feature-Skill — dieser Skill liest sie nur.

## Ausgabeformat — `<messordner>/ergebnis.md`

Feste Reihenfolge:

```
# AI Eval: <Feature>

**Datum:** <heute>
**Modus:** Messung | Schnellmodus (Tendenz, keine Freigabe) | Nur prüfen
**Feature:** <Feature-Skill>
**Eval-Datei:** <Pfad>
**Testsatz:** <Ordner>
**Läufe je Fall:** <Zahl>
**Anzahl Läufe insgesamt:** <Zahl>

## Gesamturteil
<ein Satz>

## Ergebnistabelle
| ID | Kriterium | Prüfart | Ergebnis (x von n) | vorher | Schwelle | Erreicht |
|----|-----------|---------|---------------------|--------|----------|----------|
| ... |

## Fehler zum Ansehen
### <Kriterium>
- <Fall>, Lauf <N>: <Begründung> — [Lauf-Datei](<Pfad>)

## Was die Fehler gemeinsam haben
<2–4 Sätze>

## Prüfer-Stichprobe
1. <Kriterium>, <Fall>, Lauf <N>: <Urteil + Begründung> — [Lauf-Datei](<Pfad>)
... (6 Einträge)

Prüfe diese Urteile selbst nach: eigenes Ja/Nein bilden, Übereinstimmung zählen, gegen das in der eval.md genannte Ziel halten.

## Nächster Schritt
<eine gezielte Änderung an Spec oder Regeln, dann neu messen und vergleichen>

## Hinweis zur Unabhängigkeit
<wie die Läufe erzeugt wurden und wie unabhängig sie dadurch tatsächlich sind>
```

Die Spalte „vorher" entfällt, wenn kein früheres Ergebnis existiert. Die Kopfzeile „Läufe je Fall" wird im Modus „Nur prüfen" durch die tatsächlich vorgefundene Anzahl ersetzt, die je Fall unterschiedlich sein kann.

## Qualitätskriterien

- Kriterien, Prüfart, „Wie prüfen" und Schwellen kommen ausschließlich aus der eval.md — nichts davon ist in diesem Skill hart kodiert.
- Jede Zahl (Wortgrenze, Anzahl) wird gezählt, nicht geschätzt.
- Jedes KI-Prüfer-Urteil sieht nur das, was die Frage braucht, nie die anderen Läufe.
- Das n ist für jedes Kriterium nachvollziehbar — besonders bei Kriterien, die nur beantwortete Fälle betreffen.
- „FAIL" bzw. „nicht erreicht" gilt, solange nicht wirklich gemessen wurde — nichts gilt vorab als bestanden.
- Der nächste Schritt ist eine einzelne, konkrete Änderung an Spec oder Regeln, nie an der Eval.
- Eval-Datei, Testsatz und Feature-Skill bleiben unverändert.

## Abschluss

Gib im Chat aus: das Gesamturteil, die Ergebnistabelle, den Pfad zu `ergebnis.md` und eine Zeile zum nächsten Schritt.
