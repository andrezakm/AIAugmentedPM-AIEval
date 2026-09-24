# AI Eval: Automatische Kundenmail-Antwort (Auto-Reply)

**Datum:** 2026-09-24
**Modus:** Messung
**Feature:** auto-reply (`.claude/skills/auto-reply/SKILL.md`)
**Eval-Datei:** feature/eval.md
**Testsatz:** testset/
**Läufe je Fall:** 3
**Anzahl Läufe insgesamt:** 24

## Gesamturteil
Fünf der sechs Schwellen sind erreicht — die beiden Soll-Vergleiche (E2, E3) sitzen bei 24 von 24 korrekten Entscheidungen, Form, Kernfrage und Ton liegen ebenfalls bei 15 von 15 —, aber E4 (keine erfundene Zusage) verfehlt seine Nulltoleranz-Schwelle mit einer unautorisierten Zusage in einem von 15 geprüften Läufen, sodass der Autopilot nach der Entscheidungsregel in `feature/eval.md` noch nicht eingeschaltet werden darf und ein Mensch in der Schleife bleibt.

## Ergebnistabelle
| ID | Kriterium | Prüfart | Ergebnis (x von n) | Schwelle | Erreicht |
|----|-----------|---------|---------------------|----------|----------|
| E1 | Form stimmt | Textprüfung | 15 von 15 | keine Ausnahme | Ja |
| E2 | Pflicht-Eskalation erkannt | Soll-Vergleich | 9 von 9 | keine Ausnahme (9 von 9) | Ja |
| E3 | Keine unnötige Eskalation | Soll-Vergleich | 15 von 15 | höchstens 1 Ausnahme | Ja |
| E4 | Keine erfundene Zusage | KI-Prüfer | 14 von 15 | keine Ausnahme | Nein |
| E5 | Kernfrage beantwortet | KI-Prüfer | 15 von 15 | höchstens 1 Ausnahme | Ja |
| E6 | Ton passt | KI-Prüfer | 15 von 15 | höchstens 2 Ausnahmen | Ja |

n-Herleitung: E2 aus den 9 Läufen zu den drei Soll-„eskalieren"-Mails (04, 05, 07 × 3 Läufe). E1, E3, E4, E5, E6 aus den 15 tatsächlich geschriebenen Antworten zu den fünf Soll-„beantworten"-Mails (01, 02, 03, 06, 08 × 3 Läufe) — in allen 24 Läufen stimmte die Entscheidung mit dem Soll überein, keine der 15 Antworten musste wegen einer abweichenden Eskalation aus dem n herausgerechnet werden.

## Fehler zum Ansehen
### E4 — Keine erfundene Zusage
- 01-lieferverzug, Lauf 3: Die Antwort verspricht „Sobald die Ware unser Haus verlässt, erhalten Sie eine Versandbestätigung." Weder Regel 3 (Liefertermine) noch die Auftragsdaten zu A-24831 sehen eine automatische Versandbestätigung vor — eine konkrete, für den Kunden nachprüfbare Zusage außerhalb der Regeln (Regel 8). — [Lauf-Datei](laeufe/01-lieferverzug-lauf3.md)

Für E1, E2, E3, E5 und E6 ergab die Prüfung keinen einzigen Fehler über alle 24 Läufe.

## Was die Fehler gemeinsam haben
Der einzige bestätigte Fehler entsteht dort, wo das Modell über die reine Wiedergabe der Auftragsdaten hinausgeht und von sich aus eine zusätzliche, konkret nachprüfbare Serviceleistung ankündigt (hier: eine automatische Versandbestätigung), die in keiner der acht Regeln vorgesehen ist — ein Ausrutscher in Richtung übertriebener Fürsorglichkeit statt erfundener harter Fakten wie Beträge oder Termine. Mehrere weitere Stellen zeigen die gleiche Tendenz in schwächerer, nicht als Fehler gewerteter Form — etwa „wir behalten den Auftrag im Blick" (01-lieferverzug, Lauf 2) oder „Ihren Hinweis geben wir intern weiter, damit die Ursache geklärt wird" (06-wuetende-beschwerde, alle drei Läufe): vage, nicht konkret einklagbare Zusicherungen, die ich nicht als Fehler gewertet habe, weil sie kein prüfbares Versprechen enthalten. Die Grenze zwischen harmloser Floskel und echter Zusage ist schmal, und genau an dieser Stelle würde eine Regelschärfung ansetzen.

## Prüfer-Stichprobe
1. E4, 01-lieferverzug, Lauf 3: Ja (Fehler) — verspricht eine automatische Versandbestätigung, die weder Regel 3 noch die Auftragsdaten vorsehen. — [Lauf-Datei](laeufe/01-lieferverzug-lauf3.md)
2. E4, 01-lieferverzug, Lauf 2: Nein (Grenzfall) — „behalten den Auftrag im Blick" ist vage Fürsorglichkeit ohne prüfbares Versprechen, keine Zusage im Sinne der Frage. — [Lauf-Datei](laeufe/01-lieferverzug-lauf2.md)
3. E5, 06-wuetende-beschwerde, Lauf 3: Ja — Rücksendenummer, Terminklärung nach Regel 3 und eine Entschuldigung decken „wie geht es jetzt weiter" vollständig ab, auch wenn die konkrete Zusicherung künftiger Fehlerfreiheit bewusst ausgespart bleibt. — [Lauf-Datei](laeufe/06-wuetende-beschwerde-lauf3.md)
4. E5, 03-defekt, Lauf 1: Ja — trotz der zusätzlich angebotenen Übergangslösung bei unzugänglichem Typenschild werden Fotoanfrage, Seriennummer, Gewährleistung und weiteres Vorgehen vollständig benannt. — [Lauf-Datei](laeufe/03-defekt-lauf1.md)
5. E6, 06-wuetende-beschwerde, Lauf 1: Ja — genau eine ehrliche Entschuldigung, sachlich-freundlicher Ton, keine wiederholte oder übertriebene Beschwichtigung. — [Lauf-Datei](laeufe/06-wuetende-beschwerde-lauf1.md)
6. E6, 08-zwei-fragen, Lauf 3: Ja (Grenzfall) — „Bei weiteren Fragen melden Sie sich gerne" als funktionaler, nicht werblicher Abschluss gewertet, keine Floskel im Sinne von `context/firma.md` („verzichtet auf Werbefloskeln"). — [Lauf-Datei](laeufe/08-zwei-fragen-lauf3.md)

Prüfe diese Urteile selbst nach: eigenes Ja/Nein bilden, Übereinstimmung zählen, gegen das in der eval.md genannte Ziel halten (mindestens 5 von 6 müssen übereinstimmen, sonst wird die Prüffrage geschärft, bevor mit ihr weitergemessen wird).

## Nächster Schritt
Regel 3 (Liefertermine) in `context/service-regeln.md` um einen Satz ergänzen, der zusätzliche, in den Regeln nicht vorgesehene Versand-Benachrichtigungen ausdrücklich ausschließt — etwa: „Keine weiteren Benachrichtigungen (z. B. automatische Versandbestätigungen) zusagen, die nicht Teil der Auftragsdaten oder einer anderen Regel sind." Das setzt genau an der Stelle an, an der der einzige bestätigte E4-Fehler entstanden ist. Danach neu messen (Messung 2) und das Ergebnis mit dieser Messung 1 vergleichen — bei nur 3 Läufen je Fall sind kleine Schwankungen normal und sollten nicht vorschnell als Trend gelesen werden.

## Hinweis zur Unabhängigkeit
Die 24 Läufe wurden von drei isolierten Hilfsagenten erzeugt — je ein Hilfsagent pro Lauf-Durchgang über alle 8 Mails (Lauf 1, Lauf 2, Lauf 3). Keiner der drei hatte Einblick in die Ausgaben der anderen Durchgänge, in die Eval-Datei oder in die Soll-Tabelle des Testsatzes. Die Unabhängigkeit zwischen den drei Läufen je Mail ist damit hoch; verbleibende Abhängigkeit besteht höchstens dadurch, dass jeder Hilfsagent innerhalb seines eigenen Durchgangs alle 8 Mails nacheinander bearbeitet hat und somit z. B. einen einmal gewählten Formulierungsstil über die eigenen 8 Mails hinweg mitgeschleppt haben könnte. Die Prüfung selbst (Schritt 2–9 dieses Skills) erfolgte erst nach Abschluss aller 24 Läufe und ohne dass die Läufe sich gegenseitig gesehen hätten; jedes KI-Prüfer-Urteil wurde ausschließlich anhand der jeweils einzelnen Lauf-Datei, der Mail, den Service-Regeln/Auftragsdaten und der Soll-Tabelle gebildet, nie im Vergleich zu anderen Läufen.
