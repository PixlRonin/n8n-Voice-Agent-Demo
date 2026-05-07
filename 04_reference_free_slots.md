# Referenz – Freie Demo-Termine

## Einsatz

Diese Datei dokumentiert die festen freien Termine der Demo.

Die Terminliste wird im n8n-Code Node „Prüfen und Terminlogik“ genutzt.

## Freie Termine

| Nummer | Termin |
|---|---|
| 1 | `2026-05-08 09:00` |
| 2 | `2026-05-08 10:00` |
| 3 | `2026-05-08 14:30` |
| 4 | `2026-05-09 11:00` |

## Copy-Paste-Liste für den n8n-Code Node

```javascript
const freeSlots = [
  "2026-05-08 09:00",
  "2026-05-08 10:00",
  "2026-05-08 14:30",
  "2026-05-09 11:00"
];
```

## Bedeutung für die Testfälle

| Testfall | Wunschtermin | Ergebnis |
|---|---|---|
| `booked` | `2026-05-08 10:00` | Termin ist frei |
| `missing_phone` | `2026-05-08 10:00` | Termin wäre frei, aber Telefonnummer fehlt |
| `slot_not_available` | `2026-05-08 12:00` | Termin ist nicht frei |

## Didaktischer Zweck

Die feste Terminliste macht die Demo kontrollierbar.

Teilnehmer sehen klar, warum ein Termin gebucht wird oder warum Alternativen vorgeschlagen werden.
