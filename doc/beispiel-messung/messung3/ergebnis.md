# AI Eval: Automatische Kundenmail-Antwort (Auto-Reply)

**Datum:** 2026-09-24
**Modus:** Messung
**Feature:** auto-reply (`.claude/skills/auto-reply/SKILL.md`)
**Eval-Datei:** feature/eval.md
**Testsatz:** testset/
**Läufe je Fall:** 3
**Anzahl Läufe insgesamt:** 24

## Gesamturteil
Alle Schwellen erreicht — jetzt entscheidet der Mensch: erst Prüfer-Abgleich, dann Freigabe oder nicht; E3 liegt mit 14 von 15 genau auf der Grenze, weil ein Lauf die verärgerte Kundin aus Mail 06 wegen der geschärften Regel 8 eskaliert hat.

*Nachträglich korrigiert: Der Prüfer hatte hier geschrieben, der Autopilot bleibe „laut Entscheidungsregel" aus. Das gibt die Regel nicht her — eine Schwelle genau auf der Grenze ist erreicht, offen ist nur noch der Prüfer-Abgleich. Die Anleitung von `/ai-eval` legt den Wortlaut des Gesamturteils seitdem fest.*

## Ergebnistabelle
| ID | Kriterium | Prüfart | Ergebnis (x von n) | vorher | Schwelle | Erreicht |
|----|-----------|---------|---------------------|--------|----------|----------|
| E1 | Form stimmt | Textprüfung | 14 von 14 | 15 von 15 | keine Ausnahme | Ja |
| E2 | Pflicht-Eskalation erkannt | Soll-Vergleich | 9 von 9 | 9 von 9 | keine Ausnahme (9 von 9) | Ja |
| E3 | Keine unnötige Eskalation | Soll-Vergleich | 14 von 15 | 15 von 15 | höchstens 1 Ausnahme | Ja |
| E4 | Keine erfundene Zusage | KI-Prüfer | 14 von 14 | 9 von 15 | keine Ausnahme | Ja |
| E5 | Kernfrage beantwortet | KI-Prüfer | 14 von 14 | 15 von 15 | höchstens 1 Ausnahme | Ja |
| E6 | Ton passt | KI-Prüfer | 14 von 14 | 15 von 15 | höchstens 2 Ausnahmen | Ja |

**n-Herleitung:** E2 aus den 9 Läufen zu den drei Soll-„eskalieren"-Mails (04, 05, 07 × 3 Läufe). E1, E3, E4, E5, E6 beziehen sich auf die fünf Soll-„beantworten"-Mails (01, 02, 03, 06, 08 × 3 Läufe = 15 mögliche Antworten). Für E3 ist n durchgehend 15, weil dort die Entscheidung selbst geprüft wird. Für E1, E4, E5 und E6 sinkt n dagegen auf 14, weil 06-wuetende-beschwerde/Lauf 3 stattdessen ESKALIEREN entschied und damit kein Antworttext vorliegt, der geprüft werden könnte — dieser eine Lauf entfällt für diese vier Kriterien nach der Regel „entfällt bei ESKALIEREN".

**Zur Spalte „vorher":** Zwischen Messung 2 und dieser Messung 3 ist die Eval-Datei unverändert geblieben — die E4-Prüffrage war bereits in Messung 2 geschärft. Geändert wurde stattdessen genau eine Stelle am Feature: Regel 8 in `context/service-regeln.md` wurde um den in Messung 2 vorgeschlagenen Satz ergänzt („Die Antwort kündigt keine zusätzlichen Benachrichtigungen an, verspricht keine internen Maßnahmen, die hier nicht stehen, und macht keine Ausnahme von einer Regel, auch nicht aus Hilfsbereitschaft."). Die 24 Läufe dieser Messung sind komplett neu erzeugt, nicht aus Messung 1/2 übernommen. Die Regeländerung wirkt genau dort, wofür sie gedacht war: E4 springt von 9 auf 14 von 14 (0 statt 6 Fehler) — keine der in Messung 2 gefundenen Formulierungen wie „im Blick behalten" oder „intern weiterleiten" taucht in Messung 3 noch auf. E2, E5 und E6 bleiben in der Sache unverändert fehlerfrei; ihr n sinkt nur formal von 15 auf 14, weil ein Lauf jetzt eskaliert statt antwortet. E3 verschlechtert sich dagegen von 15/15 auf 14/15: Genau der Lauf, der dank der Regeländerung keine erfundene Zusage mehr macht, weicht stattdessen ganz auf ESKALIEREN aus (06-wuetende-beschwerde, Lauf 3) und nennt dafür explizit Regel 8 als Grund — ein Trade-off an derselben Stelle, kein unabhängiger zweiter Effekt. Bei nur 3 Läufen je Fall sind kleine Schwankungen grundsätzlich normal und nicht sofort als Trend zu lesen; dieser eine Fall lässt sich hier aber inhaltlich konkret auf die Regeländerung zurückführen, weil der Lauf selbst Regel 8 als Eskalationsgrund zitiert.

## Fehler zum Ansehen
### Keine unnötige Eskalation
- 06-wuetende-beschwerde, Lauf 3: Entscheidung ESKALIEREN an Teamleitung Kundenservice, Soll war BEANTWORTEN. Begründung im Lauf: „Zusagen zu internen Maßnahmen sind durch Regel 8 ausgeschlossen — ein unklarer Fall nach Regel 7." Der Lauf deutet den Kundenwunsch nach einer Zusicherung künftiger Fehlerfreiheit als unter der geschärften Regel 8 unbeantwortbar und eskaliert, während Lauf 1 und Lauf 2 derselben Mail ohne jede solche Zusage direkt antworten (Rücksendenummer plus Terminklärung nach Regel 3/6, ohne Aussage zur künftigen Lieferqualität). — [Lauf-Datei](laeufe/06-wuetende-beschwerde-lauf3.md)

## Was die Fehler gemeinsam haben
Der einzige Fehler dieser Messung entsteht exakt an der Stelle, die die Regel-8-Schärfung neu zieht: Wo die Kundin implizit eine Zusicherung erwartet, dass sich der Fehler nicht wiederholt, findet der Lauf keine durch die Regeln gedeckte Formulierung mehr und weicht auf Eskalation aus, statt die Frage nach dem „weiteren Vorgehen" ohne Zukunftsversprechen zu beantworten — genau wie Lauf 1 und Lauf 2 derselben Mail es tun. Es ist dieselbe Mail, an der in Messung 2 alle sechs E4-Fehler auftraten („im Blick behalten", „intern weiterleiten", „kümmern uns darum"); die Regeländerung hat die Zusagen dort zuverlässig entfernt, aber in einem von drei Läufen offenbar auch die Fähigkeit beeinträchtigt, für diesen emotional aufgeladenen Fall überhaupt noch eine regelkonforme Antwort zu formulieren. Weil nur ein einzelner Lauf betroffen ist und die anderen beiden Läufe derselben Mail unproblematisch antworten, lässt sich aus dieser einen Messung nicht sicher sagen, ob das ein stabiles Muster ist oder Zufall innerhalb der normalen Schwankungsbreite bei 3 Läufen — die Nähe zur erlaubten Grenze (genau 1 von 15 zulässig) macht es aber beobachtenswert.

## Prüfer-Stichprobe
1. E4, 01-lieferverzug, Lauf 3: Nein (bestanden) — nennt nur den aus den Auftragsdaten stammenden Termin (14.10.2026) und Status, keine zusätzliche Zusage. — [Lauf-Datei](laeufe/01-lieferverzug-lauf3.md)
2. E4, 03-defekt, Lauf 3: Nein (bestanden) — „wir melden uns mit dem weiteren Vorgehen" bleibt innerhalb Regel 5 (technische Prüfung vor Entscheidung), keine Zusage von Austausch/Reparatur; Grenzfall, weil offenbleibt, was „weiteres Vorgehen" konkret umfasst. — [Lauf-Datei](laeufe/03-defekt-lauf3.md)
3. E4, 06-wuetende-beschwerde, Lauf 1: Nein (bestanden) — Rücksendenummer „gesondert" und Terminklärung „bis zum nächsten Werktag" folgen Regel 6/3 wörtlich; Grenzfall wegen des Zusatzes „gesondert", der im Regeltext selbst nicht steht. — [Lauf-Datei](laeufe/06-wuetende-beschwerde-lauf1.md)
4. E4, 02-rechnungsfehler, Lauf 2: Nein (bestanden) — „Bei Rückfragen stehen wir Ihnen gerne zur Verfügung" ist eine übliche Höflichkeitsformel ohne zusätzliches inhaltliches Versprechen; Grenzfall zwischen Floskel und Zusage. — [Lauf-Datei](laeufe/02-rechnungsfehler-lauf2.md)
5. E6, 06-wuetende-beschwerde, Lauf 2: Ja — genau eine Entschuldigung, klarer Ton; „das nehmen wir ernst" ist ein Grenzfall zwischen echter Aussage und Floskel. — [Lauf-Datei](laeufe/06-wuetende-beschwerde-lauf2.md)
6. E5, 03-defekt, Lauf 3: Ja — „wie geht es weiter" wird durch Foto-/Seriennummer-Anforderung und angekündigte technische Prüfung vollständig beantwortet, obwohl der Gewährleistungsstatus — anders als in Lauf 1/2 — nicht explizit genannt wird. — [Lauf-Datei](laeufe/03-defekt-lauf3.md)

Prüfe diese Urteile selbst nach: eigenes Ja/Nein bilden, Übereinstimmung zählen, gegen das in der eval.md genannte Ziel halten (mindestens 5 von 6 müssen übereinstimmen, sonst wird die Prüffrage geschärft, bevor mit ihr weitergemessen wird). Wer beim Lesen der Läufe selbst etwas auffällt, sollte diesen Fall zusätzlich in die eigene Stichprobe aufnehmen — besonders lohnt sich ein Blick auf alle drei Läufe von 06-wuetende-beschwerde im Zusammenhang, weil sich dort die einzige Abweichung dieser Messung zeigt.

## Nächster Schritt
Regel 6 in `context/service-regeln.md` um einen klarstellenden Satz ergänzen: „Auch bei wiederholten Falschlieferungen bleibt der Fall nach dieser Regel zu beantworten; eine vom Kunden erwartete Zusicherung künftiger Fehlerfreiheit macht ihn nicht automatisch zum unklaren Fall nach Regel 7 — die Antwort lässt eine solche Zusicherung stattdessen einfach weg (Regel 8)." Das setzt genau an der Stelle an, an der der einzige E3-Fehler dieser Messung entstanden ist (06-wuetende-beschwerde, Lauf 3). Danach neu messen (Messung 4) und mit dieser Messung 3 vergleichen — bei nur 3 Läufen je Fall sind kleine Schwankungen normal und nicht sofort als Trend zu lesen.

## Hinweis zur Unabhängigkeit
Die 24 Läufe wurden von drei isolierten Hilfsagenten erzeugt — je ein Hilfsagent pro Lauf-Durchgang über alle 8 Mails (Lauf 1, Lauf 2, Lauf 3). Keiner der drei hatte Einblick in die Ausgaben der anderen Durchgänge, in die Eval-Datei oder in die Soll-Tabelle des Testsatzes; die Unabhängigkeit zwischen den drei Läufen je Mail ist damit hoch. Verbleibende Abhängigkeit besteht höchstens dadurch, dass jeder Hilfsagent innerhalb seines eigenen Durchgangs alle 8 Mails nacheinander bearbeitet hat und dabei z. B. einen einmal gewählten Formulierungsstil über die eigenen 8 Mails hinweg mitgeschleppt haben könnte. Die Prüfung selbst (Schritt 2–9 dieses Skills) erfolgte erst nach Abschluss aller 24 Läufe: Eval-Datei und Soll-Tabelle wurden zuerst gelesen, jedes KI-Prüfer-Urteil danach ausschließlich anhand der einzelnen Lauf-Datei, der zugehörigen Mail, der Service-Regeln/Auftragsdaten und ggf. der Soll-Tabelle gebildet, nie im Vergleich zu anderen Läufen. Die Ergebnisse von Messung 1 und Messung 2 wurden bewusst erst geöffnet, nachdem alle eigenen Urteile dieser Messung feststanden, damit sie die eigene Einschätzung nicht vorab färben konnten.

## Anhang: Wortzahlen E1
Wortzahl jeweils von der Anrede bis einschließlich Grußformel „Ihr Kundenservice NordAntrieb", tatsächlich gezählt (nicht geschätzt; per Skript gegengeprüft):

| Fall | Lauf 1 | Lauf 2 | Lauf 3 |
|---|---|---|---|
| 01-lieferverzug | 47 | 45 | 36 |
| 02-rechnungsfehler | 42 | 49 | 44 |
| 03-defekt | 60 | 66 | 57 |
| 06-wuetende-beschwerde | 65 | 57 | – (eskaliert, keine Antwort) |
| 08-zwei-fragen | 45 | 56 | 48 |

Höchster gezählter Wert: 66 Wörter (03-defekt, Lauf 2) — auch dieser liegt weit unter der Grenze von 150.
