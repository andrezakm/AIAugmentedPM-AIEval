# AI Eval: Automatische Kundenmail-Antwort (Auto-Reply)

**Datum:** 2026-09-24
**Modus:** Nur prüfen
**Feature:** auto-reply (`.claude/skills/auto-reply/SKILL.md`)
**Eval-Datei:** feature/eval.md
**Testsatz:** testset/
**Läufe je Fall:** 3 (vorgefunden, für alle 8 Fälle einheitlich)
**Anzahl Läufe insgesamt:** 24

## Gesamturteil
Fünf der sechs Schwellen sind erreicht (E1, E2, E3, E5, E6 jeweils fehlerfrei), aber E4 („Keine erfundene Zusage") verfehlt seine Nulltoleranz-Schwelle deutlich — 6 von 15 geprüften Antworten enthalten mindestens eine nicht durch `context/service-regeln.md` oder die Auftragsdaten gedeckte Zusage, Ankündigung oder Regel-Ausnahme —, sodass der Autopilot nach der Entscheidungsregel in `feature/eval.md` weiterhin nicht eingeschaltet werden darf und ein Mensch in der Schleife bleibt.

## Ergebnistabelle
| ID | Kriterium | Prüfart | Ergebnis (x von n) | vorher | Schwelle | Erreicht |
|----|-----------|---------|---------------------|--------|----------|----------|
| E1 | Form stimmt | Textprüfung | 15 von 15 | 15 von 15 | keine Ausnahme | Ja |
| E2 | Pflicht-Eskalation erkannt | Soll-Vergleich | 9 von 9 | 9 von 9 | keine Ausnahme (9 von 9) | Ja |
| E3 | Keine unnötige Eskalation | Soll-Vergleich | 15 von 15 | 15 von 15 | höchstens 1 Ausnahme | Ja |
| E4 | Keine erfundene Zusage | KI-Prüfer | 9 von 15 | 14 von 15 | keine Ausnahme | Nein |
| E5 | Kernfrage beantwortet | KI-Prüfer | 15 von 15 | 15 von 15 | höchstens 1 Ausnahme | Ja |
| E6 | Ton passt | KI-Prüfer | 15 von 15 | 15 von 15 | höchstens 2 Ausnahmen | Ja |

n-Herleitung: E2 aus den 9 Läufen zu den drei Soll-„eskalieren"-Mails (04, 05, 07 × 3 Läufe). E1, E3, E4, E5, E6 aus den 15 tatsächlich geschriebenen Antworten zu den fünf Soll-„beantworten"-Mails (01, 02, 03, 06, 08 × 3 Läufe) — in allen 24 Läufen stimmte die Entscheidung mit dem Soll überein, keine der 15 Antworten musste wegen einer abweichenden Eskalation aus dem n herausgerechnet werden.

**Zur Spalte „vorher":** Zwischen Messung 1 und dieser Messung hat sich nicht das Feature verändert — es sind dieselben 24 Läufe aus `../messung1/laeufe/`, nichts wurde neu erzeugt. Geändert hat sich ausschließlich die Prüffrage von E4, die nach dem Prüfer-Abgleich zu Messung 1 geschärft wurde: Sie benennt jetzt ausdrücklich „auch Kleinigkeiten wie zusätzliche Benachrichtigungen, interne Maßnahmen oder eine Ausnahme von einer Regel" als Fehler. Fünf der sechs jetzt gezählten E4-Fehler sind exakt die Stellen, die Messung 1 in ihrem Abschnitt „Was die Fehler gemeinsam haben" bereits selbst als Grenzfälle benannt, aber mangels expliziter Nennung in der damaligen Frage nicht mitgezählt hatte („behalten den Auftrag im Blick" sowie die dreifache Formulierung „geben wir intern weiter" in Mail 06). Ein sechster Fehler (03-defekt, Lauf 1 — eine stillschweigende Ausnahme von der Seriennummer-Pflicht) wurde in Messung 1 gar nicht als E4-Fall markiert. Die Verschärfung deckt also genau das auf, wofür sie gedacht war — sie macht das Feature nicht schlechter, sie macht die Messung strenger. Bei nur 3 Läufen je Fall sind kleine Schwankungen ohnehin normal; hier handelt es sich aber nicht um Schwankung, sondern um eine bewusste Änderung des Maßstabs, weshalb der Rückgang bei E4 nicht als Regression des Features fehlgelesen werden sollte.

## Fehler zum Ansehen
### E4 — Keine erfundene Zusage
- 01-lieferverzug, Lauf 2: „Wir wissen, dass dieser Termin für Ihre Montageplanung wichtig ist, und behalten den Auftrag im Blick." — eine Zusage laufender interner Beobachtung des Auftrags, die weder Regel 3 noch die Auftragsdaten zu A-24831 vorsehen; genau die „interne Maßnahme", die die geschärfte Frage jetzt ausdrücklich erfasst. — [Lauf-Datei](../messung1/laeufe/01-lieferverzug-lauf2.md)
- 01-lieferverzug, Lauf 3: „Sobald die Ware unser Haus verlässt, erhalten Sie eine Versandbestätigung." — eine zusätzliche Benachrichtigung, die in keiner der acht Regeln und nicht in den Auftragsdaten zu A-24831 vorgesehen ist. — [Lauf-Datei](../messung1/laeufe/01-lieferverzug-lauf3.md)
- 03-defekt, Lauf 1: „Ist das Typenschild schwer zugänglich, reichen zunächst ein Foto der zugänglichen Stelle sowie weitere Angaben zur Anlage, die uns bei der Zuordnung helfen." — eine stillschweigende Ausnahme von der in Regel 5 verlangten Seriennummer, ohne dass diese Regel eine solche Ausnahme vorsieht. — [Lauf-Datei](../messung1/laeufe/03-defekt-lauf1.md)
- 06-wuetende-beschwerde, Lauf 1: „Ihren Hinweis geben wir zusätzlich intern weiter, damit die Ursache der wiederholten Fehllieferungen geklärt wird." — eine angekündigte interne Maßnahme ohne Grundlage in den Service-Regeln. — [Lauf-Datei](../messung1/laeufe/06-wuetende-beschwerde-lauf1.md)
- 06-wuetende-beschwerde, Lauf 2: „Ihren Hinweis auf die wiederholten Fehllieferungen geben wir intern weiter, damit die Ursache geprüft wird." — dieselbe Art Zusage wie Lauf 1, in eigenen Worten. — [Lauf-Datei](../messung1/laeufe/06-wuetende-beschwerde-lauf2.md)
- 06-wuetende-beschwerde, Lauf 3: „Wir nehmen Ihre Rückmeldung zum wiederholten Fehler ernst und kümmern uns intern darum." — wieder eine nicht durch die Regeln gedeckte interne Maßnahme; in allen drei Läufen dieser Mail taucht diese Formulierungsfamilie auf. — [Lauf-Datei](../messung1/laeufe/06-wuetende-beschwerde-lauf3.md)

Für E1, E2, E3, E5 und E6 ergab die Prüfung keinen einzigen Fehler über alle 24 Läufe.

## Was die Fehler gemeinsam haben
Alle sechs E4-Fehler entstehen dort, wo die Antwort über die reine Umsetzung der Regeln (Auftragsdaten wiedergeben, Rechnungskorrektur mit Frist, Rücksendenummer) hinausgeht und von sich aus entweder eine zusätzliche Handlung ankündigt (Versandbestätigung, „im Blick behalten", „intern weiterleiten", „uns darum kümmern") oder eine Nachweispflicht stillschweigend lockert (Seriennummer bei Defekt). Die Hälfte der Fehler (drei von sechs) stammt aus derselben Formulierungsfamilie in Mail 06 und tritt dort in allen drei Läufen auf — offenbar reagiert das Modell auf eine wiederholte, emotional aufgeladene Beschwerde reflexhaft mit einer zusätzlichen Fürsorge-Geste, die aber keine Regel deckt. Diese Sätze klingen einzeln harmlos und kundenfreundlich, sind aber genau die Art von unautorisierter Zusage, vor der Regel 8 warnt — nur eben in kleiner, schwer automatisch erkennbarer Form statt als erfundener Betrag oder Termin.

## Prüfer-Stichprobe
1. E4, 02-rechnungsfehler, Lauf 1: Nein (bestanden) — Prüfung, korrigierte Rechnung und Kopie sind durch Regel 4 exakt gedeckt, keine Beträge oder Gutschriften genannt. — [Lauf-Datei](../messung1/laeufe/02-rechnungsfehler-lauf1.md)
2. E4, 03-defekt, Lauf 2: Nein (bestanden) — bittet nur um die Seriennummer „sobald diese zugänglich ist", ohne eine Ausnahme von der Anforderung zu gewähren, und sagt Austausch/Reparatur nicht vorab zu. — [Lauf-Datei](../messung1/laeufe/03-defekt-lauf2.md)
3. E4, 08-zwei-fragen, Lauf 2: Nein (bestanden) — Status und Rechnungskopie kommen exakt aus den Auftragsdaten bzw. Regel 4, keine zusätzliche Zusage. — [Lauf-Datei](../messung1/laeufe/08-zwei-fragen-lauf2.md)
4. E4, 01-lieferverzug, Lauf 2: Ja (Fehler) — Grenzfall, der in Messung 1 unter der ungeschärften Frage noch als „Nein" gewertet wurde; siehe Fehlerliste oben. — [Lauf-Datei](../messung1/laeufe/01-lieferverzug-lauf2.md)
5. E5, 06-wuetende-beschwerde, Lauf 3: Ja (bestanden) — Rücksendenummer, Terminklärung nach Regel 3 und eine einmalige Entschuldigung beantworten „wie geht es jetzt weiter" vollständig. — [Lauf-Datei](../messung1/laeufe/06-wuetende-beschwerde-lauf3.md)
6. E6, 08-zwei-fragen, Lauf 1: Ja (bestanden) — klarer, sachlich-freundlicher Ton ohne Werbefloskeln, beide Fragen sauber getrennt beantwortet. — [Lauf-Datei](../messung1/laeufe/08-zwei-fragen-lauf1.md)

Prüfe diese Urteile selbst nach: eigenes Ja/Nein bilden, Übereinstimmung zählen, gegen das in der eval.md genannte Ziel halten (mindestens 5 von 6 müssen übereinstimmen, sonst wird die Prüffrage erneut geschärft, bevor mit ihr weitergemessen wird). Wer beim Lesen der Antworten selbst etwas auffällt, sollte diesen Fall zusätzlich in die eigene Stichprobe aufnehmen — insbesondere lohnt sich ein Blick auf alle drei Läufe von 06-wuetende-beschwerde im Zusammenhang, weil sich dort ein wiederkehrendes Muster zeigt.

## Nächster Schritt
Regel 8 in `context/service-regeln.md` um einen konkreten zweiten Satz ergänzen, der genau die in dieser Messung aufgetretene Fehlerart ausschließt: „Keine Ankündigung interner Maßnahmen (z. B. 'wir behalten das im Blick', 'wir leiten das intern weiter', 'wir kümmern uns intern darum') und keine Lockerung einer in diesen Regeln verlangten Angabe (z. B. der Seriennummer bei Regel 5), sofern nicht ausdrücklich durch eine der acht Regeln oder die Auftragsdaten gedeckt." Das setzt direkt an der Stelle an, an der alle sechs E4-Fehler dieser Messung entstanden sind. Danach neu messen (Messung 3) und mit dieser Messung 2 vergleichen — bei nur 3 Läufen je Fall sind kleine Schwankungen normal und nicht vorschnell als Trend zu lesen.

## Hinweis zur Unabhängigkeit
Diese Messung lief im Modus „Nur prüfen": Es wurden keine neuen Läufe erzeugt, sondern die 24 vorhandenen Lauf-Dateien aus `../messung1/laeufe/` geprüft. Deren Entstehung ist in `../messung1/ergebnis.md` dokumentiert (drei isolierte Hilfsagenten, je einer pro Lauf-Durchgang über alle 8 Mails, ohne Einblick in die anderen Durchgänge, die Eval-Datei oder die Soll-Tabelle). Für die Prüfung selbst galt zusätzlich: Die Eval-Datei und die Soll-Tabelle wurden vor jedem Urteil gelesen, aber `../messung1/ergebnis.md` wurde bewusst erst nach Abschluss aller eigenen Urteile geöffnet (siehe Vergleichsabschnitt oben), damit die frühere Bewertung die eigene Einschätzung nicht vorab gefärbt hat. Jedes KI-Prüfer-Urteil wurde ausschließlich anhand der einzelnen Lauf-Datei, der zugehörigen Mail, der Service-Regeln/Auftragsdaten und der Soll-Tabelle gebildet, nie im Vergleich zu anderen Läufen derselben oder einer anderen Mail.

## Anhang: Wortzahlen E1
Wortzahl jeweils von der Anrede bis einschließlich Grußformel „Ihr Kundenservice NordAntrieb", tatsächlich gezählt (alle Werte liegen weit unter der 150-Wörter-Grenze aus Regel 1):

| Fall | Lauf 1 | Lauf 2 | Lauf 3 |
|---|---|---|---|
| 01-lieferverzug | 53 | 53 | 65 |
| 02-rechnungsfehler | 47 | 43 | 71 |
| 03-defekt | 93 | 68 | 77 |
| 06-wuetende-beschwerde | 79 | 68 | 83 |
| 08-zwei-fragen | 48 | 42 | 57 |

Höchster gezählter Wert: 93 Wörter (03-defekt, Lauf 1) — auch dieser liegt deutlich unter der Grenze von 150.
