# Spec: Automatische Kundenmail-Antwort (Auto-Reply)

**Version:** 1.0
**Datum:** 2026-09-24
**Basis:** context/firma.md, context/service-regeln.md

---

## Zweck

Standard-Kundenmails automatisch beantworten. Zielbild ist ein Autopilot, der Routinefälle ohne menschliche Prüfung beantwortet. Aktuell befinden wir uns in der Testphase: Das Feature erzeugt ausschließlich Entwürfe, es wird nichts verschickt. Ob auf automatischen Versand umgestellt wird, entscheidet sich an den Schwellen in `feature/eval.md` — nicht an dieser Spec.

## Nutzer

Das Kundenservice-Team von NordAntrieb nutzt das Feature, um Entwürfe zu erhalten, die sie prüfen oder (im Zielbild) direkt versenden. Empfängerinnen und Empfänger der Antwort sind die Kundinnen und Kunden, die die Mail geschrieben haben.

## Input

Eine einzelne Kundenmail, gegebenenfalls mit einem Block „Auftragsdaten (aus dem System)".

## Kontext

- `context/firma.md` — Unternehmen, Kundenservice, Eskalationsrollen
- `context/service-regeln.md` — die acht Regeln, nach denen jede Antwort und jede Eskalation entschieden wird

## Output

Eine Entscheidung: **BEANTWORTEN** oder **ESKALIEREN**.

- Bei BEANTWORTEN: eine fertige Antwortmail nach den Regeln aus `context/service-regeln.md`.
- Bei ESKALIEREN: an wen (Rolle laut `context/firma.md`), eine Begründung mit Bezug auf die betreffende Regelnummer, und eine Kurzfassung für den Menschen, der den Fall übernimmt.

## Constraints

- Jede Antwort und jede Eskalation folgt den acht Regeln aus `context/service-regeln.md`, ohne Ausnahme.
- Keine Zusagen außerhalb dieser Regeln — auch nicht, wenn die Mail freundlich oder dringend formuliert ist.
- Das Feature verschickt nie etwas und behauptet nie, etwas verschickt zu haben. Es erzeugt ausschließlich einen Entwurf.

## Sonderfälle

- **Mehrere Fragen in einer Mail:** alle Fragen einzeln beantworten, keine übersehen.
- **Fehlende Auftragsdaten:** keinen Termin, keine Zahl und keinen Status erfinden — stattdessen die dafür vorgesehene Formulierung aus den Regeln verwenden oder, wenn die Regeln das verlangen, eskalieren.
- **Unklare oder nicht eindeutig zuordenbare Mail:** eskalieren statt raten.
