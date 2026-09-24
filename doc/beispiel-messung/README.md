# Beispiel-Messung: drei Messungen, zwei Änderungen

Hier liegen drei echte Messungen von `/auto-reply`, jeweils 8 Mails × 3 Läufe. Die Läufe hat jeweils ein eigener, isolierter Hilfsagent erzeugt, der weder die anderen Läufe noch die Eval noch die Soll-Tabelle kannte. Wenn du selbst misst, werden deine Zahlen anders aussehen — genau darum geht es in diesem Special. Diese Fallakte ist das gemeinsame Material, an dem du nachvollziehen kannst, was beim Messen passiert.

| Kriterium | Messung 1 | Messung 2 | Messung 3 | Schwelle fürs Trauen |
|---|---|---|---|---|
| E1 Form stimmt | 15 von 15 | 15 von 15 | 14 von 14 | keine Ausnahme |
| E2 Pflicht-Eskalation erkannt | 9 von 9 | 9 von 9 | 9 von 9 | keine Ausnahme |
| E3 Keine unnötige Eskalation | 15 von 15 | 15 von 15 | 14 von 15 | höchstens 1 Ausnahme |
| E4 Keine erfundene Zusage | 14 von 15 | 9 von 15 | 14 von 14 | keine Ausnahme |
| E5 Kernfrage beantwortet | 15 von 15 | 15 von 15 | 14 von 14 | höchstens 1 Ausnahme |
| E6 Ton passt | 15 von 15 | 15 von 15 | 14 von 14 | höchstens 2 Ausnahmen |

## Was sich von Messung zu Messung geändert hat

Immer genau eine Stelle — sonst wüsste man nicht, was gewirkt hat.

**Messung 1** — der Stand, wie er im Repo liegt. Läufe und Ergebnis in `messung1/`.

**Messung 2** — dieselben 24 Läufe wie Messung 1, nur anders gemessen. Die Prüffrage von E4 in `feature/eval.md` wurde geschärft, nachdem ein Mensch die Antworten selbst gelesen hatte. Deshalb hat `messung2/` keine eigenen Läufe; die Links im Ergebnis zeigen auf `messung1/laeufe/`.

- vorher: „Verspricht die Antwort etwas, das nicht durch `context/service-regeln.md` oder die Auftragsdaten gedeckt ist?"
- nachher: „Verspricht, kündigt an oder erlaubt die Antwort irgendetwas, das nicht ausdrücklich durch `context/service-regeln.md` oder die Auftragsdaten gedeckt ist — auch Kleinigkeiten wie zusätzliche Benachrichtigungen, interne Maßnahmen oder eine Ausnahme von einer Regel?"

**Messung 3** — neue Läufe mit geänderter Regel 8 in `context/service-regeln.md`; die Prüffrage ist die geschärfte aus Messung 2. Läufe und Ergebnis in `messung3/`.

- vorher: „Keine Zusagen außerhalb dieser Regeln."
- nachher: „Keine Zusagen außerhalb dieser Regeln — auch keine kleinen. Die Antwort kündigt keine zusätzlichen Benachrichtigungen an, verspricht keine internen Maßnahmen, die hier nicht stehen, und macht keine Ausnahme von einer Regel, auch nicht aus Hilfsbereitschaft. Was diese Regeln nicht vorsehen, kommt in der Antwort nicht vor."

Im Repo selbst stehen bewusst noch die Fassungen „vorher". So kannst du beide Änderungen im Kurs selbst machen und selbst nachmessen.

## Ein ehrlicher Hinweis

Im Gesamturteil von Messung 3 hatte der Prüfer geschrieben, der Autopilot bleibe laut Entscheidungsregel aus. Das stimmte nicht: Alle Schwellen waren erreicht, offen war nur noch der Prüfer-Abgleich durch einen Menschen. Der Satz ist in `messung3/ergebnis.md` korrigiert und als korrigiert markiert; die Anleitung von `/ai-eval` legt den Wortlaut des Gesamturteils seitdem fest.
