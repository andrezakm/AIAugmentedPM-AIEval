# Auto-Reply

Nimmt eine einzelne Kundenmail entgegen und entscheidet zuerst: BEANTWORTEN oder ESKALIEREN. Bei BEANTWORTEN schreibst du einen fertigen Antwortentwurf, bei ESKALIEREN eine Notiz mit Adressat und Begründung. Du befindest dich in der Testphase — es wird nie etwas verschickt, nur ein Entwurf geschrieben. Ob und wann später wirklich automatisch verschickt wird, entscheidet keine einzelne Antwort, sondern eine Messung über viele Läufe (siehe `/ai-eval`).

## Aufruf

```
/auto-reply testset/mails/07-anwalt.md
/auto-reply <eingefügte Mail>
/auto-reply
```

Ohne Angabe fragt der Skill kurz nach, welche Mail gemeint ist, und nennt die vorhandenen Dateien unter `testset/mails/` zur Auswahl.

## Liest

- `context/firma.md`, `context/service-regeln.md`, `feature/spec.md` — immer, vollständig.
- Die angegebene oder eingefügte Mail, einschließlich eines möglichen Blocks „Auftragsdaten (aus dem System)".
- **Nicht** `feature/eval.md` — das Feature kennt seine eigene Abnahmeprüfung nicht.

## Schreibt

`output/YYYY-MM-DD/auto-reply/<mail-id>-lauf<N>.md` — eine Datei pro Lauf, N zählt hoch, wenn du dieselbe Mail mehrfach durchspielst.

## Beispiel

`/auto-reply testset/mails/03-defekt.md` — eine Mail über einen brummenden Getriebemotor nach acht Monaten. Der Skill entscheidet BEANTWORTEN, fordert Fotos und Seriennummer an und sagt keinen Austausch vorab zu, weil die Regeln das erst nach technischer Prüfung erlauben.

## Grenzen

Ein KI-Feature antwortet bei derselben Mail nicht jedes Mal gleich — ein einzelner Lauf zeigt dir eine mögliche Antwort, keine Garantie. Ob das Feature insgesamt verlässlich genug ist, sagt dir nicht dieser Skill, sondern `/ai-eval`, das viele Läufe gegeneinander hält. Im Zweifel eskaliert dieser Skill lieber einmal zu oft als zu wenig — das ist Absicht, keine Schwäche.
