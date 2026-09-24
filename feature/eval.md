# Eval: Automatische Kundenmail-Antwort (Auto-Reply)

**Datum:** 2026-09-24
**Basis:** feature/spec.md

---

## Warum diese Eval anders aussieht als in Woche 4

In Woche 4 hat jedes Kriterium einmal PASS oder FAIL bekommen — eine Ausgabe, ein Urteil. Eine KI-Antwort auf dieselbe Mail fällt bei jedem Lauf etwas anders aus: gleiche Eingabe, unterschiedliche Ausgabe. Ein einzelner Lauf sagt deshalb wenig darüber, ob wir dem Feature trauen können. Gemessen wird deshalb über einen Testsatz (mehrere Mails) und mehrere Läufe pro Mail, das Ergebnis ist eine Quote: „x von n".

**Standard-Messung: 8 Mails × 3 Läufe = 24 Läufe.**

E1, E4, E5 und E6 werden über alle tatsächlich geschriebenen Antworten zu Mails mit Soll „beantworten" gemessen (n = Anzahl dieser Antworten, im Normalfall 5 Mails × 3 Läufe = 15).

Auch wie streng eine Schwelle ist, ist keine feste Regel, sondern eine Entscheidung des Produkts: Wer eine übersehene Pflicht-Eskalation für schlimmer hält als eine überflüssige, setzt dort auch eine strengere Schwelle als anderswo.

## Drei Prüfarten, von einfach nach aufwendig

- **Textprüfung** prüft eine feste Regel direkt im Text — kommt ein Wort, eine Zahl oder ein Format vor oder nicht.
- **Soll-Vergleich** vergleicht die getroffene Entscheidung mit einem vorher festgelegten richtigen Wert aus der Soll-Tabelle in `testset/README.md`.
- **KI-Prüfer** stellt eine einzelne Ja/Nein-Frage, dort wo tatsächlich ein Urteil nötig ist, das sich nicht mechanisch feststellen lässt.

Für jedes Kriterium wird immer die einfachste Prüfart genommen, die ausreicht: Eine Textprüfung ist eindeutig nachvollziehbar und kostet nichts, ein Soll-Vergleich ist genauso eindeutig, und ein KI-Prüfer kommt nur zum Einsatz, wo wirklich ein Urteil gefragt ist — und genau deshalb muss man diesem Urteil zuerst selbst über die Schulter schauen (siehe Prüfer-Abgleich unten).

## Kriterien

| ID | Kriterium | Prüfart | Wie prüfen | Schwelle fürs Trauen | Ergebnis |
|---|---|---|---|---|---|
| E1 | Form stimmt | Textprüfung | Anrede mit Namen; enthält die Mail eine Auftrags- oder Rechnungsnummer, kommt sie in der Antwort vor; Grußformel „Ihr Kundenservice NordAntrieb"; ≤ 150 Wörter | keine Ausnahme | FAIL |
| E2 | Pflicht-Eskalation erkannt | Soll-Vergleich | Mails mit Soll „eskalieren" → Entscheidung ESKALIEREN (3 Mails × 3 Läufe = 9) | keine Ausnahme (9 von 9) | FAIL |
| E3 | Keine unnötige Eskalation | Soll-Vergleich | Mails mit Soll „beantworten" → Entscheidung BEANTWORTEN (5 × 3 = 15) | höchstens 1 Ausnahme | FAIL |
| E4 | Keine erfundene Zusage | KI-Prüfer (Ja/Nein) | „Verspricht die Antwort etwas, das nicht durch `context/service-regeln.md` oder die Auftragsdaten gedeckt ist?" (Ja = Fehler) | keine Ausnahme | FAIL |
| E5 | Kernfrage beantwortet | KI-Prüfer (Ja/Nein) | „Beantwortet die Antwort die Kernfrage aus `testset/README.md` vollständig?" | höchstens 1 Ausnahme | FAIL |
| E6 | Ton passt | KI-Prüfer (Ja/Nein) | „Ist die Antwort freundlich, klar und ohne Floskeln — und entschuldigt sie sich bei einer berechtigten Beschwerde genau einmal?" | höchstens 2 Ausnahmen | FAIL |

## „FAIL" heißt: noch nicht bestanden

„FAIL" bedeutet hier: noch nicht gemessen oder die Schwelle noch nicht erreicht. Wie in Woche 4 gilt nichts als bestanden, bevor tatsächlich gemessen wurde — die Tabelle startet deshalb vollständig auf FAIL.

## Prüfer-Abgleich

Bevor wir den KI-Prüfer-Kriterien E4 bis E6 vertrauen, prüft ein Mensch eine Stichprobe von 6 Prüfer-Urteilen selbst anhand der Lauf-Dateien nach. Mindestens 5 von 6 müssen mit dem eigenen Urteil übereinstimmen — sonst wird die Prüffrage geschärft, bevor mit ihr weitergemessen wird. In die Stichprobe gehören ausdrücklich auch bestandene Urteile: Gefährlich ist vor allem der Fehler, den der Prüfer durchgelassen hat. Und wenn dir beim Lesen der Antworten selbst etwas auffällt, gehört genau dieser Fall in die Stichprobe.

## Entscheidungsregel

Der Autopilot wird nur eingeschaltet, wenn alle sechs Schwellen erreicht sind **und** der Prüfer-Abgleich stimmt. Solange das nicht der Fall ist, bleibt ein Mensch in der Schleife: Fehler ansehen, Spec oder Regeln genau an der Stelle schärfen, an der die Fehler entstehen, danach neu messen.

## Warum die Schwellen unterschiedlich streng sind

E2 und E4 haben „keine Ausnahme", weil die dahinterliegenden Fehler teuer sind: eine übersehene Anwaltsdrohung, die unbeaufsichtigt weiterläuft, oder eine erfundene Gutschrift, auf die sich ein Kunde später berufen kann. E6 ist lockerer, weil ein leicht unpassender Ton ärgerlich, aber folgenlos ist — hier reichen zwei Ausnahmen, bevor an der Formulierung nachgeschärft wird.
