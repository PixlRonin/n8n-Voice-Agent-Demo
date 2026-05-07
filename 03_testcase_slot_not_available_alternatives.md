# Testfall 3 – Wunschtermin nicht frei

## Einsatz

Dieser Datensatz wird genutzt, um einen belegten oder nicht verfügbaren Termin zu zeigen:

Der Kunde nennt alle notwendigen Informationen, aber der gewünschte Termin steht nicht in der Liste der freien Demo-Termine.

## TEST_CASE

```javascript
const TEST_CASE = "slot_not_available";
```

## Didaktischer Zweck

Die Teilnehmer sehen, dass n8n nicht nur Daten prüft, sondern auch eine einfache Terminlogik ausführt.

Wenn der Wunschtermin nicht frei ist, werden Alternativtermine zurückgegeben.

## Eingangsdaten vom Mock Voice Agent

| Feld | Wert |
|---|---|
| `call_id` | `demo-call-003` |
| `customer_name` | `Ali Demir` |
| `phone` | `+491711112222` |
| `email` | `ali.demir@example.com` |
| `service` | `Ersttermin` |
| `preferred_date` | `2026-05-08` |
| `preferred_time` | `12:00` |
| `duration_minutes` | `30` |
| `notes` | `Kunde möchte einen Termin um 12 Uhr.` |
| `consent` | `true` |
| `transcript` | `Kunde fragt nach einem Ersttermin am Freitag um 12 Uhr.` |

## Copy-Paste-Datensatz als JSON

```json
{
  "call_id": "demo-call-003",
  "customer_name": "Ali Demir",
  "phone": "+491711112222",
  "email": "ali.demir@example.com",
  "service": "Ersttermin",
  "preferred_date": "2026-05-08",
  "preferred_time": "12:00",
  "duration_minutes": 30,
  "notes": "Kunde möchte einen Termin um 12 Uhr.",
  "consent": true,
  "transcript": "Kunde fragt nach einem Ersttermin am Freitag um 12 Uhr."
}
```

## Erwartete Prüfung in n8n

| Prüfschritt | Ergebnis |
|---|---|
| Pflichtdaten vorhanden | Ja |
| Telefonnummer vorhanden | Ja |
| Einwilligung vorhanden | Ja |
| Leistung erlaubt | Ja |
| Wunschtermin frei | Nein |

## Erwartetes Ergebnis nach der n8n-Terminlogik

| Feld | Wert |
|---|---|
| `status` | `slot_not_available` |
| `requested_slot` | `2026-05-08 12:00` |
| `booked_slot` | leer |
| `alternative_slots` | `2026-05-08 09:00`, `2026-05-08 10:00`, `2026-05-08 14:30` |
| `errors` | leer |

## Erwarteter Antworttext für den Voice Agent

```text
Der gewünschte Termin ist leider nicht frei. Ich kann alternativ folgende Termine anbieten: 2026-05-08 09:00, 2026-05-08 10:00, 2026-05-08 14:30.
```

## Erwarteter Google-Sheet-Eintrag

Im Google Sheet sollte eine neue Zeile entstehen mit:

| Spalte | Erwarteter Wert |
|---|---|
| `call_id` | `demo-call-003` |
| `customer_name` | `Ali Demir` |
| `phone` | `+491711112222` |
| `email` | `ali.demir@example.com` |
| `service` | `Ersttermin` |
| `preferred_date` | `2026-05-08` |
| `preferred_time` | `12:00` |
| `duration_minutes` | `30` |
| `status` | `slot_not_available` |
| `booked_slot` | leer |
| `alternative_slots` | `2026-05-08 09:00 | 2026-05-08 10:00 | 2026-05-08 14:30` |
| `errors` | leer |

## Erklärung für Teilnehmer

Dieser Testfall zeigt den Unterschied zwischen gültigen Daten und verfügbarem Termin.

Die Kundendaten sind vollständig. Trotzdem wird nicht gebucht, weil der gewünschte Termin nicht frei ist.

n8n gibt stattdessen Alternativen zurück.
