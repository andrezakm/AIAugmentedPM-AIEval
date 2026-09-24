---
name: kurs
description: Interaktiver Kurs zum Special "Trauen wir uns den Autopiloten?" — führt Schritt für Schritt durch vier Aha-Momente zu KI-Eval an einem Kundenservice-Feature (NordAntrieb, fiktiv): erst vorhersagen, dann auflösen. Schritt N entspricht Lektion N in der Kursplattform. Aufrufen mit `/kurs`, wenn dieser Kurs durchlaufen werden soll.
allowed-tools: Read, Write, Glob
---

# Kurs: Trauen wir uns den Autopiloten?

## Beim Start — deine erste Nachricht

Beginne nie direkt mit Schritt 1. Wenn der Kurs aufgerufen wird, ist deine erste Nachricht der Einstieg, und danach wartest du:

1. **Worum es geht**, in zwei, drei Sätzen (Abschnitt unten): NordAntrieb (fiktiv), der Autopilot als Ziel, die Testphase, die Frage „Trauen wir uns?".
2. **Wie der Kurs funktioniert:** vier Schritte, jeder entspricht einer Lektion in der Kursplattform; Navigation mit „weiter", „Schritt 3", „zurück", „Übersicht".
3. **Wie die Fragen gemeint sind.** Erklär ausdrücklich: In jedem Schritt kommt an einer Stelle eine Frage, bevor aufgelöst wird. Das ist kein Test, es gibt keine Note. Die Teilnehmenden bilden sich zuerst ein eigenes Urteil und halten es fest, weil die Einsicht jedes Schritts genau im Unterschied zwischen ihrem ersten Eindruck und der Auflösung liegt. Wo es einen Maßstab gibt, meist die Service-Regeln, steht er bei der Frage dabei.
4. **Zeitrahmen** in einem Satz.
5. **Die vier Schritte** als kurze Liste.

Schließ mit der Frage, ob es losgehen kann. Erst auf „los", „weiter" oder etwas Gleichbedeutendes beginnt Schritt 1 — und zwar mit dem Abschnitt „Der Fall", nicht mit der ersten Antwort.

## Worum es geht

NordAntrieb GmbH ist eine fiktive Firma, eigens für dieses Special erfunden: rund 120 Kundenmails am Tag, und die Geschäftsführung möchte im Ziel einen Autopiloten, der Standardanfragen automatisch beantwortet. Aktuell läuft eine Testphase — das Feature schreibt nur Entwürfe, verschickt wird nichts. Die Frage, um die sich dieser Kurs dreht, ist die der Geschäftsführung: Trauen wir uns, irgendwann auf automatischen Versand umzustellen? Die Kriterien dafür stehen in **feature/eval.md**.

Dieser Kurs ist die interaktive Fassung von vier Lektionen aus der Kursplattform: **Schritt N hier entspricht Lektion N dort**, gleiche Reihenfolge, gleiche Beispiele, gleiche Zitate — nur spielst du sie hier live im Repo durch, statt sie nur zu lesen.

**Zeitrahmen, ehrlich:** Die vier Schritte mit der Fallakte (den drei echten Messungen unter `doc/beispiel-messung/`) dauern zusammen etwa 60 bis 90 Minuten. Eigene Messungen kommen on top: Der Schnellmodus (`/ai-eval schnell`) ist kurz, eine volle Standard-Messung (`/ai-eval`, 24 Läufe) kann eine Weile brauchen. Niemand muss selbst messen, um die vier Aha-Momente zu erleben — die Fallakte allein liefert sie bereits.

**Navigation:** Die Teilnehmenden steuern den Kurs mit kurzen Zurufen:
- „weiter" — nächster Schritt
- „Schritt 3" (oder eine andere Zahl) — direkt zu diesem Schritt springen
- „zurück" — einen Schritt zurück
- „Übersicht" — die Tabelle unten noch einmal zeigen

**Die vier Schritte:**

| Schritt | Titel | Worum es geht |
|---|---|---|
| 1 | Einmal hinsehen beweist nichts | Warum eine einzelne Antwort nichts beweist |
| 2 | Was heißt gut genug? | Die sechs Kriterien der Eval und ihre Schwellen |
| 3 | Sei der Prüfer | Dem KI-Prüfer selbst über die Schulter schauen |
| 4 | Nachschärfen hat einen Preis | Eine Regel schärfen — und was das an anderer Stelle kostet |

## Regeln für dich als Agent

Diese Regeln gelten für den ganzen Kurs, nicht nur für einzelne Schritte:

1. **Immer nur ein Schritt auf einmal.** Führe nie zwei Schritte in einer Antwort aus. Schließe jeden Schritt mit einer Zusammenfassung in genau einem Satz und einer Einladung zu „weiter" ab.
2. **Die harte Regel des Kurses — Vorhersage vor Auflösung.** Jeder Schritt hat eine Stelle, an der eine Frage gestellt wird, bevor die Antwort fällt. Stelle die Frage, dann stoppe ausdrücklich und warte auf die Antwort der Teilnehmenden. Die Auflösung steht weiter unten im jeweiligen Schritt — zeig sie unter keinen Umständen, bevor die Teilnehmenden geantwortet haben. Wenn sie die Vorhersage ausdrücklich überspringen wollen, ist das in Ordnung: Sag kurz, dass der Aha-Moment davon lebt, selbst zu antworten, und lös dann auf. Nenn bei jeder Frage, **was** beurteilt wird und **woran** (den Maßstab). Nach der Antwort ordnest du sie zuerst konkret ein — was daran stimmt, was die Auflösung ergänzt —, bevor du auflöst; nie als falsch abkanzeln.
3. **Zitiere, statt zu kippen.** Die referenzierten Dateien sind teils lang. Lies sie mit Read, aber gib im Chat nur das wörtlich wieder, was für den jeweiligen Punkt zählt — nicht die ganze Datei.
4. **Nichts ändern, außer ausdrücklich verlangt.** `feature/eval.md`, alles unter `context/` und alles unter `testset/` bleiben unverändert, solange die Teilnehmenden dich nicht ausdrücklich darum bitten. Das ist in Schritt 3 (Prüffrage E4) und Schritt 4 (Regel 8) vorgesehen — dort sagt der Kurs es dir. Überall sonst: nur zeigen, nie ändern.
5. **Nie etwas verschicken.** Das Feature bleibt in der Testphase, es entstehen ausschließlich Entwürfe.
6. **Fachbegriffe beim ersten Auftreten kurz erklären**, unter anderem: nicht-deterministisch, Testsatz, Lauf, Eval, Schwelle, KI-Prüfer, eskalieren.

---

## Schritt 1 — Einmal hinsehen beweist nichts

### Der Fall

Bevor irgendeine Antwort beurteilt wird, brauchen die Teilnehmenden den Fall. Stell ihn in dieser Reihenfolge vor, knapp und im Gesprächston:

1. **Das Feature.** Es bekommt eine einzelne Kundenmail, manchmal mit einem Block Auftragsdaten aus dem System, und trifft zuerst eine Entscheidung: beantworten oder **eskalieren**, also den Fall an eine zuständige Stelle im Haus weitergeben (etwa Rechtsabteilung oder technischer Support). Erst danach schreibt es einen Antwortentwurf oder eine kurze Notiz für den Menschen, der den Fall übernimmt. Verschickt wird nichts.
2. **Die Wissensbasis.** Lies **context/service-regeln.md** und fasse die acht Regeln in je einer Zeile zusammen — Form, Ton, Liefertermine nur aus den Auftragsdaten, Rechnungen (Korrektur und Kopie zusagbar, innerhalb von drei Werktagen, keine Beträge oder Gutschriften), Defekt (Fotos und Seriennummer, Austausch erst nach Prüfung), Rückgabe (30 Tage, danach Kulanz; Falschlieferung mit Rücksendenummer), Pflicht-Eskalation, und Regel 8 „Keine Zusagen außerhalb dieser Regeln" im Wortlaut.
3. **Der Testsatz.** Acht Kundenmails unter **testset/mails/**, in einem Satz aufgezählt: verspätete Lieferung, doppelt berechnete Rechnungsposition, brummender Getriebemotor, Rückgabe nach 45 Tagen, technische Frage zu einem Frequenzumrichter, verärgerte Beschwerde über die dritte Fehllieferung, Produktionsausfall, zwei Fragen in einer Mail. **Öffne dafür nicht testset/README.md** — dort stehen die richtigen Antworten und die Tücken, das würde spätere Aha-Momente vorwegnehmen.
4. **Die erste Mail.** Lies **testset/mails/01-lieferverzug.md** und zeig sie wörtlich, einschließlich des Blocks „Auftragsdaten (aus dem System)".

**Kurzer Halt:** Frag, ob der Fall klar ist oder ob die Teilnehmenden eine Regel oder eine andere Mail genauer sehen möchten. Geh weiter, sobald sie bereit sind.

### Die erste Antwort

Zeig jetzt eine echte, saubere Antwort des Features auf genau diese Mail. Lies **doc/beispiel-messung/messung1/laeufe/01-lieferverzug-lauf1.md** und zitiere den Antworttext wörtlich:

```
Sehr geehrter Herr Reimers,

vielen Dank für Ihre Nachricht zu Auftrag A-24831. Der Auftrag befindet sich aktuell in
Fertigung, der Versand wird vorbereitet. Der Liefertermin laut unserem System ist der
14.10.2026.

Wir hoffen, dass Ihnen das für Ihre interne Planung weiterhilft. Bei weiteren Fragen
melden Sie sich gerne.

Freundliche Grüße
Ihr Kundenservice NordAntrieb
```

**Vorhersage-Frage:** „Würdest du diese Antwort so an Herrn Reimers schicken lassen, ohne dass vorher ein Mensch draufschaut? Miss sie an den acht Regeln von eben — ja oder nein, mit einem Satz Begründung."

**HALTEPUNKT — warte auf die Antwort, bevor du weitermachst.**

**Auflösung.** Ordne zuerst die Antwort der Teilnehmenden ein: Gemessen an den Regeln ist diese Antwort in Ordnung — wer Ja gesagt hat, hat sie richtig beurteilt. Wer im Schlusssatz („Wir hoffen, dass Ihnen das … weiterhilft") eine Floskel sieht, hat ebenfalls einen Punkt; über solche Grenzfälle lässt sich streiten, das kommt in Schritt 3. Dann der Haken, der nicht in dieser Antwort liegt: Dieselbe Mail wurde dem Feature insgesamt dreimal gegeben. Lies **doc/beispiel-messung/messung1/laeufe/01-lieferverzug-lauf3.md** — im dritten Lauf steht zusätzlich dieser Satz:

```
Sobald die Ware unser Haus verlässt, erhalten Sie eine Versandbestätigung.
```

Ordne ein: Diese Zusage steht in keiner der acht Service-Regeln (`context/service-regeln.md`) — niemand hat eine automatische Versandbestätigung versprochen, das Feature hat sie sich in diesem einen Lauf einfach ausgedacht. Weise darauf hin, woher der Satz kommt: Herr Reimers erwähnt in seiner Mail nebenbei, dass er bisher keine Versandbestätigung bekommen hat, und das Feature wollte hilfsbereit darauf eingehen. Lauf 1 und Lauf 2 zur selben Mail enthalten den Satz nicht. Genauso bei Mail 03, dem brummenden Getriebemotor: Nur Lauf 1 lockert die Regel zur Seriennummer und bietet bei schwer zugänglichem Typenschild vorerst ein Foto der zugänglichen Stelle als Ersatz an. Jeder dieser Ausrutscher tauchte in genau einem von drei Läufen auf.

**Kernsatz.** Ein KI-Feature antwortet bei gleicher Eingabe jedes Mal etwas anders — das nennt man **nicht-deterministisch**: anders als ein klassisches Programm, das bei derselben Eingabe garantiert dasselbe liefert, schreibt ein KI-Feature zur selben Mail unterschiedliche Antworten, selbst wenn man es jedes Mal genau gleich fragt. Ein einzelner Blick auf eine Antwort sagt deshalb wenig darüber, ob man dem Feature trauen kann. Gemessen wird deshalb über einen **Testsatz** (eine feste Sammlung von Testfällen mit vorher bekannter richtiger Antwort) und mehrere **Läufe** (dieselbe Mail wird mehrmals hintereinander gegeben) — das Ergebnis ist keine einzelne Note, sondern eine Quote, „x von n".

**Beruhigung.** In allen 24 Läufen dieser ersten Messung hat das Feature die große Entscheidung — beantworten oder **eskalieren**, also den Fall an eine zuständige Stelle weiterreichen — jedes Mal richtig getroffen. Die Ausrutscher stecken ausschließlich in den Details der Formulierung.

**Selbst-Ausprobieren.** Bitte die Teilnehmenden, im Chat zweimal genau denselben Befehl selbst einzutippen:

```
/auto-reply testset/mails/06-wuetende-beschwerde.md
```

Sag ihnen, dass danach zwei Dateien unter **output/\<heutiges Datum\>/auto-reply/** liegen — eine pro Lauf — und dass sie „weiter" sagen sollen, sobald beide Läufe fertig sind. Wenn sie das tun, ermittle die beiden Dateien per Glob (Muster `output/*/auto-reply/06-wuetende-beschwerde-lauf*.md`, den jüngsten Tagesordner nehmen), lies beide und hilf beim Vergleich Satz für Satz: Wo sind sie gleich, wo weichen sie voneinander ab?

**Abschluss Schritt 1:** Ein Satz wie „Eine einzelne Antwort beweist nichts, weil dieselbe Mail beim nächsten Mal anders beantwortet wird — deshalb misst man über mehrere Läufe." Lade zu „weiter" für Schritt 2 ein.

---

## Schritt 2 — Was heißt gut genug?

**Vorhersage-Frage:** „Wie viele Fehler darf ein Autopilot im Kundenservice machen? Leg dich auf eine Zahl fest, etwa ‚einer von hundert Mails' — eine richtige Zahl gibt es nicht, aber deine eigene brauchst du gleich."

**HALTEPUNKT — warte auf die Antwort, bevor du weitermachst.**

**Auflösung.** Die ehrliche Antwort: Kommt darauf an, welcher Fehler. Eine übersehene Anwaltsdrohung wiegt anders als ein Satz, der ein bisschen zu förmlich klingt.

**feature/eval.md gemeinsam durchgehen.** Lies die Datei vollständig und geh die sechs Kriterien durch:

- **E1, Form stimmt** — Textprüfung: Anrede mit Namen, eine Auftrags- oder Rechnungsnummer aus der Mail kommt vor, Grußformel „Ihr Kundenservice NordAntrieb", höchstens 150 Wörter. Schwelle: keine Ausnahme.
- **E2, Pflicht-Eskalation erkannt** — Soll-Vergleich: Mails mit Soll „eskalieren" müssen auch tatsächlich eskaliert werden. Schwelle: keine Ausnahme (9 von 9).
- **E3, keine unnötige Eskalation** — ebenfalls Soll-Vergleich, umgekehrt: Mails mit Soll „beantworten" sollen auch beantwortet werden. Schwelle: höchstens 1 Ausnahme.
- **E4, keine erfundene Zusage** — KI-Prüfer: „Verspricht die Antwort etwas, das nicht durch `context/service-regeln.md` oder die Auftragsdaten gedeckt ist?" Schwelle: keine Ausnahme.
- **E5, Kernfrage beantwortet** — KI-Prüfer: ob die Antwort die eigentliche Frage vollständig beantwortet. Schwelle: höchstens 1 Ausnahme.
- **E6, Ton passt** — KI-Prüfer: ob die Antwort freundlich und klar ist und sich bei einer berechtigten Beschwerde genau einmal entschuldigt. Schwelle: höchstens 2 Ausnahmen.

Erkläre die drei Prüfarten, von einfach nach aufwendig: **Textprüfung** (eine feste Regel wird direkt im Text nachgesehen, kostet nichts), **Soll-Vergleich** (die richtige Antwort steht vorher fest), **KI-Prüfer** (eine einzelne Ja/Nein-Frage, dort wo wirklich ein Urteil nötig ist, das sich nicht mechanisch feststellen lässt). Für jedes Kriterium gilt: die einfachste Prüfart, die ausreicht — und weil der KI-Prüfer selbst eine KI ist, irrt er auch mal (dazu mehr in Schritt 3).

Nenne die Grafik **doc/eval-schleife.png** — die Teilnehmenden können sie im Datei-Explorer öffnen oder im Browser ansehen.

**Warum die Schwellen unterschiedlich streng sind.** E2 und E4 dürfen nie vorkommen: eine übersehene Anwaltsdrohung, die unbeaufsichtigt weiterläuft, oder eine erfundene Zusage, auf die sich ein Kunde später beruft, sind teuer. E3 höchstens einmal, E6 höchstens zweimal — eine unnötig eskalierte Mail kostet einen Menschen ein paar Minuten, ein leicht unpassender Ton ist ärgerlich, beides richtet aber keinen echten Schaden an. Der Unterschied zwischen den Schwellen ist der Schaden im Ernstfall — eine Entscheidung des Produkts, nicht ein fester Wert von außen.

**Entscheidungsregel:** Der Autopilot wird nur eingeschaltet, wenn alle sechs Schwellen erreicht sind **und** zusätzlich ein Mensch eine Stichprobe der Urteile des KI-Prüfers selbst nachgeprüft hat (wie das konkret abläuft, kommt in Schritt 3).

**Zwei kurze Geschichten** (je ein, zwei Sätze):
- Beim Formulieren der Testmails fiel auf, dass eine Mail nach einer Frist für eine Rechnungskorrektur fragt, die Service-Regeln damals aber keine Frist kannten — ein Widerspruch im eigenen Testsatz, gefunden vor der ersten Messung, gelöst durch eine feste Frist in Regel 4. Merksatz: Fehleranalyse beginnt beim eigenen Testsatz, nicht erst beim Feature.
- Das Feature, das die Mails beantwortet, liest weder die eval.md noch die Soll-Tabelle, und die Messung lässt erst alle Antworten schreiben, bevor sie überhaupt die Kriterien liest — wer die Klausurfragen vorher kennt, lernt auf die Klausur hin statt auf das eigentliche Thema.

**Übung.** Bitte die Teilnehmenden, **feature/eval.md** selbst zu öffnen und eine Schwelle zu finden, die sie anders setzen würden, mit einer kurzen Begründung, die sich ausschließlich auf den möglichen Schaden bezieht — **nur notieren, die Datei bleibt unverändert**, sonst wären spätere Messungen nicht mehr mit der Fallakte vergleichbar. Reagiere auf ihre Begründung: Frag nach, ob sie wirklich am Schaden hängt oder eher am Bauchgefühl, und spiegle, was das für die Entscheidungsregel bedeuten würde.

**Abschluss Schritt 2:** Ein Satz wie „Was gut genug ist, legt niemand von außen fest, sondern eine Entscheidung über den möglichen Schaden je Kriterium." Lade zu „weiter" für Schritt 3 ein.

---

## Schritt 3 — Sei der Prüfer

**Ausgangslage.** Die erste Messung (acht Testmails, je drei Läufe) kam zu einem knappen Nein: Bei E4 fand der KI-Prüfer genau einen Fehler in 15 geprüften Antworten — den Satz mit der Versandbestätigung aus Schritt 1. Seine Stichprobe von sechs Urteilen stimmte dabei 6 von 6 mit einem Menschen überein, ein scheinbar sehr verlässlicher Prüfer.

Zitiere aus **context/service-regeln.md** die drei Regeln, die für die folgende Übung zählen:

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

Zeig die fünf Sätze der Übung — jeder stammt wörtlich aus einer Antwort der ersten Messung:

```
a) Sobald die Ware unser Haus verlässt, erhalten Sie eine
   Versandbestätigung. (Mail 01, Lauf 3)

b) Ist das Typenschild schwer zugänglich, reichen zunächst ein Foto
   der zugänglichen Stelle sowie weitere Angaben zur Anlage, die uns
   bei der Zuordnung helfen. (Mail 03, Lauf 1)

c) Ihren Hinweis geben wir zusätzlich intern weiter, damit die
   Ursache der wiederholten Fehllieferungen geklärt wird.
   (Mail 06, Lauf 1)

d) Wir nehmen Ihre Rückmeldung zum wiederholten Fehler ernst und
   kümmern uns intern darum. (Mail 06, Lauf 3)

e) Eine korrigierte Rechnung erhalten Sie von uns innerhalb von
   3 Werktagen. (Mail 02, Lauf 2)
```

**Vorhersage-Frage:** „Beurteile die fünf Sätze für dich: Verspricht der Satz etwas, das diese Regeln nicht decken — ja oder nein? Jeweils einzeln."

**HALTEPUNKT — warte auf alle fünf Urteile, bevor du auflöst.** Nimm auch entgegen, wenn nur ein Teil der fünf beantwortet wird, aber weise freundlich darauf hin, dass der Vergleich gleich für alle fünf gilt.

**Auflösung.** Der Prüfer der ersten Messung hat nur a) als Fehler gezählt. c) und d) hat er gesehen und ausdrücklich durchgehen lassen, weil sie „kein prüfbares Versprechen" enthielten. b) hat er gar nicht erst als Zusage erkannt — bei der Prüfung der Kernfrage notierte er den Satz sogar als „zusätzlich angebotene Übergangslösung" und winkte ihn durch. Ein Mensch, der die Regeln oben ernst nimmt, zählt dagegen a) bis d) als Fehler: Keine der vier Zusagen — automatische Benachrichtigung, gelockerte Nachweispflicht, zwei Varianten einer versprochenen internen Maßnahme — steht in einer der acht Regeln. e) dagegen ist eine echte Grauzone: Regel 4 sagt „gehen … raus", die Antwort sagt „erhalten Sie" — Versand ist nicht dasselbe wie Eingang beim Kunden. Sag das den Teilnehmenden ehrlich so: Beide Prüfer, der ursprüngliche wie der später geschärfte, ließen diesen Satz gelten, darüber lässt sich streiten.

Ordne jedes der fünf Urteile der Teilnehmenden einzeln gegen diese Auflösung ein — sag konkret, bei welchem Satz sie mit dem Prüfer übereinstimmen und bei welchem mit dem Menschen, der genauer hinschaut.

**Einsicht 1 — die Stichprobe.** 6 von 6 Übereinstimmung, und trotzdem hat der Prüfer in derselben Messung Fehler durchgelassen — drei davon haben die Teilnehmenden gerade selbst gefunden, und die geschärfte Prüfung (gleich) findet noch zwei weitere. Der Grund: Der Prüfer hatte seine Stichprobe selbst ausgewählt, seine eigenen blinden Flecken kamen darin naturgemäß nicht vor. Konsequenz: Die Stichprobe nicht dem Prüfer überlassen — bestandene Urteile aus dem strengsten Kriterium hineinnehmen, denn dort verstecken sich die durchgelassenen Fehler, und genau die Fälle, die einem selbst beim Lesen aufgefallen sind.

**Einsicht 2 — die gefährlichen Fehler sind die freundlichen.** Keiner der vier Fehler war eine große Erfindung, kein erfundener Betrag, kein erfundener Termin. Alle vier waren hilfsbereite Kleinigkeiten — eine zusätzliche Benachrichtigung, eine stillschweigende Ausnahme, eine interne Maßnahme, die niemand zugesagt hatte. Genau solche Sätze überliest man am leichtesten.

**Messung 2 — das Messgerät eichen.** Die Prüffrage von E4 wurde geschärft (Wortlaut vorher/nachher steht in **doc/beispiel-messung/README.md**, biete ihn bei Bedarf an). Mit der geschärften Frage wurden dieselben 24 Antworten aus Messung 1 neu geprüft, ohne dass ein einziger neuer Lauf entstand: E4 fällt von 14 auf 9 von 15 — sechs Fehler statt einem, nämlich a) bis d) und zwei weitere derselben Art. Das Feature ist dadurch nicht schlechter geworden, es wird jetzt nur ehrlich gemessen. Merksatz: erst das Messgerät, dann das Feature.

**Optional — eigene Messung.** Wer möchte, misst selbst:

```
/ai-eval schnell
```

oder für eine belastbare Messung:

```
/ai-eval
```

Hilf danach, das eigene **ergebnis.md** zu finden und zu lesen (Glob-Muster `output/*/ai-eval/messung*/ergebnis.md`, den jüngsten nehmen) und die eigenen Prüfer-Urteile mit der Prüfer-Stichprobe darin abzugleichen.

**Optional — Prüffrage E4 selbst schärfen.** Im Repo steht noch die ursprüngliche Fassung von E4 in **feature/eval.md**. Biete den geschärften Wortlaut aus **doc/beispiel-messung/README.md** an — die Teilnehmenden ändern die Datei entweder selbst, oder bitten dich ausdrücklich darum, das für sie zu tun (nur dann darfst du `feature/eval.md` ändern). Danach:

```
/ai-eval nur prüfen output/<Datum>/ai-eval/messung1/laeufe/
```

Ermittle den Pfad zur eigenen ersten Messung per Glob (`output/*/ai-eval/messung1/laeufe/`), bevor du ihn nennst.

**Hinweis:** Eigene Zahlen weichen von der Fallakte ab — genau das ist der Punkt, wegen desselben Nicht-Determinismus aus Schritt 1.

**Abschluss Schritt 3:** Ein Satz wie „Ein Prüfer, der sich selbst nie widerspricht, hat vielleicht nur seine eigenen blinden Flecken bestätigt." Lade zu „weiter" für Schritt 4 ein.

---

## Schritt 4 — Nachschärfen hat einen Preis

**Vorher/nachher an Regel 8.** Zitiere die aktuelle Fassung aus **context/service-regeln.md**:

```
vorher:
Keine Zusagen außerhalb dieser Regeln.
```

Und die geschärfte Fassung aus **doc/beispiel-messung/README.md**:

```
nachher:
Keine Zusagen außerhalb dieser Regeln — auch keine kleinen. Die Antwort
kündigt keine zusätzlichen Benachrichtigungen an, verspricht keine internen
Maßnahmen, die hier nicht stehen, und macht keine Ausnahme von einer Regel,
auch nicht aus Hilfsbereitschaft. Was diese Regeln nicht vorsehen, kommt in
der Antwort nicht vor.
```

**Die Parallele:** Regel 8 hat Zusagen außerhalb der Regeln schon immer verboten — aber das Feature hat „Zusage" genauso eng gelesen wie der Prüfer in der ersten Messung, nämlich als großes, nachprüfbares Versprechen. Der Prüfer selbst hatte nach Messung 1 nur eine schmale Korrektur vorgeschlagen: den einen Satz zur Versandbestätigung zu verbieten. Das hätte den einen gefundenen Fall behoben, aber nicht die ganze Familie ähnlicher Kleinigkeiten aus Schritt 3. Merksatz: KI-Vorschläge reparieren oft nur das Symptom, das Muster erkennt man selbst.

**Vorhersage-Frage:** „Was passiert, wenn wir Regel 8 verschärfen? Denk an beide Seiten — die erfundenen Zusagen und die Entscheidung zwischen beantworten und eskalieren — und leg dich fest."

**HALTEPUNKT — warte auf die Antwort, bevor du auflöst.**

**Auflösung Messung 3.** Neue Läufe, geschärfte Regel 8, zwei gegenläufige Effekte: E4 springt von 9 von 15 auf 14 von 14 — keine der kleinen Zusagen aus Schritt 3 taucht wieder auf. Aber E3 sinkt von 15 von 15 auf 14 von 15: Ein Lauf eskaliert die verärgerte Kundin aus Mail 06 an die Teamleitung, statt zu antworten, und begründet das ausdrücklich mit der neuen Regel. Zitiere aus **doc/beispiel-messung/messung3/laeufe/06-wuetende-beschwerde-lauf3.md**:

```
Zusagen zu internen Maßnahmen sind durch Regel 8 ausgeschlossen — ein
unklarer Fall nach Regel 7.
```

Ordne ein: Mehr Strenge an einer Stelle macht das Feature an einer anderen vorsichtiger — dort, wo es vorher eine unautorisierte Zusage gemacht hätte, weicht es jetzt lieber ganz auf eine Eskalation aus. Genau für diesen Effekt lässt E3 eine Ausnahme zu, eine Schwelle, die vor dieser Messung festgelegt wurde, nicht danach passend gemacht.

Nenne die Grafik **doc/drei-messungen.png**.

**Die Entscheidung.** Rechnerisch sind jetzt alle sechs Schwellen erreicht. Der Prüfer schrieb im Ergebnis trotzdem, der Autopilot bleibe „laut Entscheidungsregel" aus — das stimmte nicht, denn die Regel verlangt nur noch den Prüfer-Abgleich durch einen Menschen; das Werkzeug hat sich vor der Entscheidung gedrückt (inzwischen korrigiert, siehe **doc/beispiel-messung/README.md**). Die Freigabe trifft ein Mensch. Bitte die Teilnehmenden, für sich die Freigabe-Frage zu beantworten: Würden sie an dieser Stelle den Autopiloten einschalten — vollständig, teilweise oder noch nicht — und warum genau an diesem Punkt? Wenn sie antworten, spiegle die zwei Wege aus der Lektion, ohne eine einzig richtige Antwort vorzugeben:
- den Prüfer-Abgleich aus Schritt 3 tatsächlich machen und den Autopiloten zunächst nur für die eindeutigsten Standardfälle einschalten — Lieferstatus mit vorhandenen Auftragsdaten, Zusendung einer Rechnungskopie;
- oder den Testsatz mit echten Mails aus dem laufenden Betrieb erweitern, sodass jeder tatsächlich aufgetretene Fehler eine neue Testmail wird.

Ergänze den ehrlichen Hinweis: Acht Testmails für 120 echte Mails am Tag sind wenig. Die Eval macht diese Entscheidung besprechbar und nachvollziehbar — sie nimmt sie niemandem ab.

**Optional.** Wer möchte, schärft Regel 8 in **context/service-regeln.md** selbst — mit dem Wortlaut von oben oder einer eigenen Formulierung, nur wenn die Teilnehmenden das ausdrücklich wünschen oder dich ausdrücklich darum bitten — misst neu und vergleicht das Ergebnis mit der vorigen Messung.

**Übertrag als Mini-Werkstatt.** Frag die Teilnehmenden nach einem KI-Feature aus ihrem eigenen Arbeitsalltag. Skizziere mit ihnen im Chat, ohne irgendetwas als Datei anzulegen (außer sie wünschen es ausdrücklich), die drei Teile:
1. Was das Feature tut, kurz.
2. Drei Beispielfälle für einen Testsatz mit bekanntem Soll — mindestens einer davon mit einer Tücke, also einer Stelle, an der das Feature typischerweise stolpern könnte.
3. Drei Kriterien, jeweils mit Prüfart (Textprüfung, Soll-Vergleich oder KI-Prüfer) und Schwelle, mit einer Begründung, die sich auf den möglichen Schaden bezieht.

**Abschluss Schritt 4:** Ein Satz wie „Nachschärfen an einer Stelle wirkt fast nie nur an dieser einen Stelle." Lade zu „weiter" für den Abschluss ein.

---

## Abschluss

Fass die vier Merksätze in je einem Satz zusammen:

1. Ein KI-Feature antwortet bei gleicher Eingabe jedes Mal etwas anders — ein einzelner Blick beweist nichts, gemessen wird über Testsatz und Läufe.
2. Was „gut genug" bedeutet, legt das Produkt fest, mit Schwellen, die sich am möglichen Schaden orientieren, nicht am Bauchgefühl.
3. Bevor man dem Feature misstraut oder vertraut, eicht man das Messgerät — auch der KI-Prüfer irrt.
4. Nachschärfen hat einen Preis: mehr Strenge an einer Stelle macht ein Feature an anderer Stelle vorsichtiger.

Weise zum Schluss darauf hin, dass **/ai-eval** generisch funktioniert: für jedes eigene KI-Feature mit eigenem Feature-Skill, eigenem Testsatz und eigener `eval.md` — Details dazu stehen in **.claude/skills/ai-eval/README.md**.
