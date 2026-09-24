---
name: kurs
description: Interaktiver Kurs zum Special über Evals für AI-Features in sechs Schritten, synchron zu den sechs Lektionen in der Kursplattform. Aufrufen mit /kurs, um das Special Schritt für Schritt durchzugehen.
---

> Für den Agenten: Zeig diesen Kurs Schritt für Schritt und gib den Text jedes Abschnitts vollständig wieder, bis zum nächsten „Sag ‚weiter'". Dann wartest du. Nicht zusammenfassen, nicht vorgreifen.

# Kurs: Trauen wir uns den Autopiloten?

Willkommen zum Special.

Im Spec-Eval-Pattern haben wir uns bisher angeschaut, wie wir deterministische Features beschreiben und testen lassen — Features, die bei derselben Eingabe immer dasselbe tun. Für nicht-deterministische Features klappt das nicht. Gemeint sind Features, deren Output zum Beispiel von einem Sprachmodell, einem LLM, erzeugt wird; oft heißen sie einfach AI-Features. Weil ihr Output bei jedem Aufruf anders ausfallen kann, müssen wir sie anders angehen.

In diesem Special zeigen wir, wie das geht. Der Kern ist eine andere eval.md: eine, die Erwartungen an einen nicht-deterministischen Output formuliert. Statt eine Antwort einmal anzusehen und PASS oder FAIL zu vergeben, lässt sie viele Fälle mehrfach durchlaufen, zählt, wie oft jede Erwartung erfüllt ist, und legt vorher fest, ab welcher Quote wir dem Feature trauen. Wo ein Urteil nötig ist, prüft eine zweite KI mit — und ein Mensch prüft diesen Prüfer.

Die Spec bleibt dabei, was sie in Woche 4 war: die Beschreibung, was das Feature tun soll. Neu sind die Eval und die Art, wie getestet wird — erst von dir, dann von einer KI. Das Ganze machst du an einem Beispiel: einem AI-Feature, das Kundenmails beantwortet.

**Das Beispiel:** Die NordAntrieb GmbH (fiktiv) bekommt rund 120 Kundenmails am Tag. Die Geschäftsführung möchte, dass eine KI Standardmails irgendwann automatisch beantwortet — ein Autopilot. Noch sind wir in der Testphase, die KI schreibt nur Entwürfe, verschickt wird nichts. **Deine Rolle:** Du sollst entscheiden, ob wir uns den Autopiloten trauen.

Der Weg ist derselbe wie in Woche 4:

1. **Das Feature** — worum es geht und welche Dateien es gibt
2. **Die Spec** — was das Feature tun soll
3. **Die Eval** — wann wir ihm trauen, und warum sie anders aussieht als in Woche 4
4. **Der Mensch testet** — du lässt das Feature laufen und prüfst selbst
5. **Die KI testet** — ein Agent misst, und du prüfst den Prüfer
6. **Nachschärfen und entscheiden**

Jeder Schritt entspricht der gleichnamigen Lektion in der Kursplattform. Rechne mit etwa 90 Minuten, plus Wartezeit für die Messungen in Schritt 5 und 6. Du navigierst mit „weiter" oder „Schritt 3". Frag jederzeit nach, wenn ein Begriff unklar ist.

Sag „los", wenn es losgehen kann.

---

## Schritt 1: Das Feature

Das Feature bekommt eine Kundenmail, manchmal zusammen mit Auftragsdaten aus dem System, und entscheidet zuerst: **beantworten** oder **eskalieren**. Eskalieren heißt, die Mail an einen Menschen im Haus weiterzugeben, etwa an die Rechtsabteilung oder den technischen Support. Erst danach schreibt es einen Antwortentwurf oder eine kurze Notiz für die Person, die den Fall übernimmt.

Das sind die Dateien, mit denen du arbeitest:

| Datei | Was es ist |
|---|---|
| `context/firma.md` | die Firma und die Stellen, an die eskaliert wird |
| `context/service-regeln.md` | acht Regeln, nach denen das Feature antwortet — seine Wissensbasis |
| `feature/spec.md` | was das Feature tun soll (Schritt 2) |
| `feature/eval.md` | wann wir ihm trauen (Schritt 3) |
| `testset/` | acht Testmails, jeweils mit der richtigen Entscheidung (Schritt 3) |
| `/auto-reply` | das Feature selbst — du rufst es in Schritt 4 auf |
| `/ai-eval` | die Messung — du rufst sie in Schritt 5 auf |
| `doc/beispiel-messung/` | drei echte Messungen, mit denen du deine eigenen Ergebnisse vergleichst |

Öffne `context/service-regeln.md` im Explorer links und lies die acht Regeln. Merk dir besonders die letzte, Regel 8: „Keine Zusagen außerhalb dieser Regeln." Sie wird noch wichtig.

**Reflexion:** Wo gibt es in deinem Arbeitsalltag Anfragen, die zu großen Teilen Routine sind? Woran würdest du festmachen, ob eine KI sie beantworten darf?

Sag „weiter" für Schritt 2.

---

## Schritt 2: Die Spec

Öffne `feature/spec.md`. Wie in Woche 4 beschreibt die Spec, **was** gebaut wird, nicht wie: Zweck, Nutzer, Input, Kontext, Output, Constraints und Sonderfälle.

Fragen beim Lesen:
- Was genau liefert das Feature, wenn es beantwortet — und was, wenn es eskaliert?
- Welche Constraints sollen verhindern, dass es Kunden etwas verspricht, was die Firma nicht halten kann?
- Wer entscheidet laut Spec, ob der Autopilot eingeschaltet wird? (Kleiner Hinweis: nicht die Spec.)

Ein wichtiger Unterschied zu Woche 4: Ein Prototyp tut, was die Spec sagt, und zwar jedes Mal gleich. Eine KI *versucht*, es zu tun, und formuliert jede Antwort neu. Ob sie sich zuverlässig an die Spec hält, steht nirgends in der Spec — das lässt sich nur messen. Deshalb braucht ein KI-Feature eine andere Eval. Um die geht es in Schritt 3.

Übrigens: Das Feature `/auto-reply` liest die Spec und die Regeln, aber absichtlich nicht die Eval und nicht die richtigen Antworten. Wer die Klausurfragen vorher kennt, lernt auf die Klausur hin.

**Reflexion:** Könntest du aus der Spec vorhersagen, wie die Antwort an eine wütende Kundin genau klingt? Wo lässt die Spec dem Feature Spielraum?

Sag „weiter" für Schritt 3.

---

## Schritt 3: Die Eval

Das ist der Kern des Specials. In Woche 4 sah eine Zeile der Eval so aus:

| ID | Kriterium | Wie testen | Pass-Bedingung | Ergebnis |
|---|---|---|---|---|
| E3 | Jede Karte zeigt den Cluster-Namen | Karten durchsehen | Jede Karte hat eine Überschrift mit dem Namen | PASS oder FAIL |

Einmal hinsehen genügte, weil der Prototyp immer gleich reagiert. Das Feature hier antwortet auf dieselbe Mail jedes Mal anders — ein PASS beim ersten Mal sagt nichts über das zweite Mal. Deshalb ändert sich die Eval an fünf Stellen:

| In Woche 4 | Hier | Warum |
|---|---|---|
| ein Fall | ein **Testsatz**: acht Mails, für jede steht die richtige Entscheidung vorher fest (das **Soll**) | ein einzelner Fall kann zufällig gut ausgehen |
| ein Durchgang | mehrere **Läufe**: jede Mail dreimal, also 24 Antworten | dieselbe Mail wird jedes Mal anders beantwortet |
| PASS oder FAIL | eine **Quote**: „x von n" Antworten erfüllen das Kriterium | 24 Antworten fallen nicht alle gleich aus |
| eine Pass-Bedingung | eine **Schwelle** pro Kriterium, je nach möglichem Schaden unterschiedlich streng | eine Quote ist nicht von sich aus gut oder schlecht |
| „durchsehen" | eine **Prüfart** pro Kriterium; wo ein Urteil nötig ist, prüft eine zweite KI (der **KI-Prüfer**), und ein Mensch prüft den Prüfer per Stichprobe | 24 Antworten prüft niemand von Hand — und auch der KI-Prüfer kann irren |

Öffne jetzt `feature/eval.md`. Ganz oben steht genau diese Begründung. Darunter kommen die sechs Kriterien:

- **E1 Form stimmt** — Textprüfung (Anrede, Nummer, Grußformel, Wortzahl) — keine Ausnahme erlaubt
- **E2 Pflicht-Eskalation erkannt** — Soll-Vergleich — keine Ausnahme
- **E3 Keine unnötige Eskalation** — Soll-Vergleich — höchstens 1 Ausnahme
- **E4 Keine erfundene Zusage** — KI-Prüfer — keine Ausnahme
- **E5 Kernfrage beantwortet** — KI-Prüfer — höchstens 1 Ausnahme
- **E6 Ton passt** — KI-Prüfer — höchstens 2 Ausnahmen

Warum so unterschiedlich streng? Eine übersehene Anwaltsdrohung (E2) oder eine erfundene Gutschrift (E4) kann teuer werden. Eine unnötig eskalierte Mail kostet einen Menschen ein paar Minuten. Wie streng eine Schwelle ist, legt also nicht das Werkzeug fest, sondern das Produkt. Und die Entscheidungsregel am Ende der Datei sagt: Der Autopilot wird nur eingeschaltet, wenn alle Schwellen erreicht sind **und** ein Mensch die Urteile des KI-Prüfers stichprobenartig bestätigt hat.

Öffne auch `testset/README.md`. Dort stehen die acht Mails, jeweils mit Soll und mit der „Tücke" — der Stelle, an der das Feature stolpern könnte. Die Grafik `doc/eval-schleife.png` zeigt den ganzen Ablauf auf einen Blick.

**Reflexion:** Welche Schwelle würdest du anders setzen — und mit welchem möglichen Schaden begründest du das? (Nur notieren, die Datei bleibt, wie sie ist.)

Sag „weiter" für Schritt 4.

---

## Schritt 4: Der Mensch testet

Wie in Woche 4 prüfst du zuerst selbst, bevor ein Agent prüft. Nur so kannst du später beurteilen, ob du dem Agenten trauen kannst.

Öffne `testset/mails/01-lieferverzug.md` und lies die Mail samt Auftragsdaten. Dann führe diesen Befehl **zweimal** aus:

```
/auto-reply testset/mails/01-lieferverzug.md
```

Die beiden Antworten liegen danach im Ordner `output/<heutiges Datum>/auto-reply/`. Prüf beide selbst gegen die sechs Kriterien: Stimmt die Form? Hat das Feature richtig entschieden? Verspricht es etwas, das keine Regel deckt? Ist die Frage beantwortet? Passt der Ton? Und: Sind die beiden Antworten gleich? Würdest du sie so verschicken?

Sag „weiter", wenn du fertig bist.

### Teil 2

Dieselbe Mail lief in unserer ersten echten Messung dreimal. Lauf 1 und Lauf 2 waren sauber. Lauf 3 enthielt diesen Satz:

```
Sobald die Ware unser Haus verlässt, erhalten Sie eine Versandbestätigung.
```

Keine Regel sieht das vor. Das Feature wollte hilfsbereit darauf eingehen, dass der Kunde in seiner Mail erwähnt, bisher keine Versandbestätigung bekommen zu haben. Hättest du nur Lauf 1 gesehen, wäre dir nichts aufgefallen. **Einmal hinsehen beweist nichts.**

Jetzt bist du der Prüfer. Hier drei Regeln im Wortlaut:

```
Regel 4 (Rechnungen): Prüfung, korrigierte Rechnung und Zusendung einer
Rechnungskopie zusagen ist erlaubt. Korrigierte Rechnungen und Rechnungskopien
gehen innerhalb von 3 Werktagen raus. Keine Beträge, Gutschriften oder
Erstattungen zusagen.

Regel 5 (Defekt/Gewährleistung): 24 Monate Gewährleistung ab Lieferung. Bei
Defekt Fotos und Seriennummer anfordern; Austausch oder Reparatur erst nach
technischer Prüfung — nie vorab zusagen.

Regel 8: Keine Zusagen außerhalb dieser Regeln.
```

Und fünf Sätze aus Antworten der ersten Messung:

```
a) Sobald die Ware unser Haus verlässt, erhalten Sie eine
   Versandbestätigung. (Mail 01, Lauf 3)

b) Ist das Typenschild schwer zugänglich, reichen zunächst ein Foto
   der zugänglichen Stelle sowie weitere Angaben zur Anlage, die uns
   bei der Zuordnung helfen. (Mail 03, Lauf 1)

c) Ihren Hinweis geben wir zusätzlich intern weiter, damit die
   Ursache der wiederholten Fehllieferungen geklärt wird. (Mail 06, Lauf 1)

d) Wir nehmen Ihre Rückmeldung zum wiederholten Fehler ernst und
   kümmern uns intern darum. (Mail 06, Lauf 3)

e) Eine korrigierte Rechnung erhalten Sie von uns innerhalb von
   3 Werktagen. (Mail 02, Lauf 2)
```

Entscheide für jeden Satz: Verspricht er etwas, das die Regeln nicht decken — ja oder nein? Schreib deine fünf Urteile hier in den Chat. Du brauchst sie in Schritt 5.

Sag „weiter", wenn du deine Urteile notiert hast.

### Teil 3

An den Regeln gemessen sind a) bis d) Zusagen ohne Deckung: eine Benachrichtigung, die niemand vorgesehen hat, eine gelockerte Pflicht zur Seriennummer, zweimal eine versprochene interne Maßnahme. e) ist eine Grauzone: Regel 4 sagt „gehen raus", die Antwort sagt „erhalten Sie" — Versand ist nicht Eingang. Darüber kann man streiten.

Auffällig ist: Keiner dieser Fehler ist eine große Erfindung, kein falscher Betrag, kein erfundener Termin. Es sind freundliche Kleinigkeiten, und genau die überliest man. **Die gefährlichen Fehler sind die freundlichen.**

**Reflexion:** Wie lange hast du für fünf Sätze gebraucht? Was hieße das für 120 Mails am Tag?

Sag „weiter" für Schritt 5.

---

## Schritt 5: Die KI testet

Jetzt übernimmt ein Agent die Arbeit, die du gerade von Hand gemacht hast — für den ganzen Testsatz. Das ist der Befehl `/ai-eval`. Er macht fünf Dinge nacheinander:

1. Er lässt `/auto-reply` jede Testmail beantworten, ohne dabei die Eval zu kennen.
2. Danach liest er `feature/eval.md` und die richtigen Entscheidungen aus `testset/README.md`.
3. Er prüft jede Antwort gegen jedes Kriterium, mit der Prüfart, die in der Eval steht — für E4 bis E6 als KI-Prüfer, der eine Ja/Nein-Frage beantwortet und kurz begründet.
4. Er zählt pro Kriterium „x von n" und vergleicht mit der Schwelle.
5. Er schreibt alles in eine Datei `ergebnis.md`: Gesamturteil, Tabelle, jeden gefundenen Fehler und eine Stichprobe von KI-Prüfer-Urteilen, die du selbst nachprüfen sollst.

Führe ihn aus, im Schnellmodus mit einer Antwort pro Mail. Das dauert einige Minuten:

```
/ai-eval schnell
```

(Die volle Messung mit drei Läufen pro Mail startest du mit `/ai-eval` ohne Zusatz. Sie dauert entsprechend länger.)

Öffne danach dein Ergebnis unter `output/<heutiges Datum>/ai-eval/messung1/ergebnis.md`. Schau dir die Tabelle an und vergleiche das Urteil des KI-Prüfers zu E4 mit deinen fünf Urteilen aus Schritt 4.

Sag „weiter", wenn du es gelesen hast.

### Teil 2

Deine Zahlen weichen von unseren ab — bei einem nicht-deterministischen Feature ist das normal. In unserer ersten echten Messung (`doc/beispiel-messung/messung1/ergebnis.md`, drei Läufe pro Mail) sah es so aus: Der KI-Prüfer hat bei E4 nur Satz a) als Fehler gezählt, also 14 von 15. Die Sätze c) und d) hat er gesehen und durchgelassen, weil sie „kein prüfbares Versprechen" enthielten. Satz b) hat er gar nicht als Zusage erkannt. Und seine Stichprobe von sechs Urteilen stimmte trotzdem 6 von 6 mit einem Menschen überein — weil er sie selbst ausgesucht hatte.

Die Lehre: **Prüf den Prüfer, und such die Stichprobe selbst aus.** Deshalb wurde die Prüffrage von E4 geschärft. Sie nennt jetzt ausdrücklich auch kleine Zusagen, interne Maßnahmen und Ausnahmen von Regeln. Mit der geschärften Frage wurden dieselben 24 Antworten noch einmal geprüft: E4 fiel von 14 auf 9 von 15. Das Feature war nicht schlechter geworden, es wurde nur ehrlich gemessen. **Erst das Messgerät, dann das Feature.** Den Wortlaut vorher und nachher findest du in `doc/beispiel-messung/README.md`.

**Reflexion:** Wem vertraust du — dir, dem KI-Prüfer, der Stichprobe? Was müsste passieren, damit du die Prüfung von Hand weglässt?

Sag „weiter" für Schritt 6.

---

## Schritt 6: Nachschärfen und entscheiden

Die kleinen Zusagen verstoßen eigentlich schon gegen Regel 8. Aber das Feature hat „Zusage" genauso eng gelesen wie der Prüfer — als großes, nachprüfbares Versprechen. Also schärfst du jetzt das Feature selbst. Öffne `context/service-regeln.md` und ersetze Regel 8 durch diesen Text (oder bitte mich, das für dich zu tun):

```
8. **Keine Zusagen außerhalb dieser Regeln — auch keine kleinen.** Die Antwort
kündigt keine zusätzlichen Benachrichtigungen an, verspricht keine internen
Maßnahmen, die hier nicht stehen, und macht keine Ausnahme von einer Regel,
auch nicht aus Hilfsbereitschaft. Was diese Regeln nicht vorsehen, kommt in
der Antwort nicht vor.
```

Bevor du neu misst: Was glaubst du, passiert jetzt? Denk an beide Seiten — die erfundenen Zusagen und die Entscheidung zwischen beantworten und eskalieren.

Dann miss neu:

```
/ai-eval schnell
```

Die neue Messung landet in `messung2` und wird automatisch mit deiner ersten verglichen.

In unserer echten dritten Messung (`doc/beispiel-messung/messung3/`) verschwanden die kleinen Zusagen ganz, E4 stand bei 14 von 14. Dafür hat das Feature einmal die verärgerte Kundin aus Mail 06 eskaliert, statt ihr zu antworten, und sich dabei ausdrücklich auf die neue Regel 8 berufen. **Mehr Strenge an einer Stelle macht das Feature an einer anderen vorsichtiger.** Genau für so etwas erlaubt E3 eine Ausnahme. Die Grafik `doc/drei-messungen.png` zeigt alle drei Messungen nebeneinander.

Damit waren alle Schwellen erreicht. Die Entscheidung trifft trotzdem ein Mensch, keine Messung — und acht Testmails für 120 Mails am Tag sind wenig.

**Deine Entscheidung:** Würdest du den Autopiloten einschalten — ganz, teilweise (zum Beispiel nur für Lieferstatus und Rechnungskopien) oder noch nicht? Und warum?

**Zum Mitnehmen:** `/ai-eval` funktioniert für jedes KI-Feature, nicht nur für NordAntrieb. Du brauchst dieselben drei Teile wie hier: eine Anleitung für das Feature (wie `/auto-reply`), einen Testsatz mit Soll (wie `testset/`) und eine Eval (wie `feature/eval.md`). Dann rufst du den Befehl mit deinen eigenen Pfaden auf, zum Beispiel:

```
/ai-eval Feature: mein-feature, Testsatz: meine-faelle/, Eval: meine-eval.md
```

Wie man die drei Teile anlegt, steht in `.claude/skills/ai-eval/README.md`.

**Reflexion:** Welches KI-Feature aus deinem Alltag würdest du so prüfen wollen? Was wären drei Testfälle dafür — und einer davon mit einer Tücke?

Das war das Special. Danke fürs Mitmachen.
