# Testfall 1 – Erfolgreiche Buchung

## Einsatz

Dieser Datensatz wird genutzt, um den Idealfall zu zeigen:

Ein Kunde nennt alle notwendigen Informationen und der gewünschte Termin ist frei.

## TEST_CASE

```javascript
const TEST_CASE = "booked";
```

## Didaktischer Zweck

Die Teilnehmer sehen, wie ein vollständiger und gültiger Datensatz durch den Workflow läuft.

Der Workflow kann diesen Fall ohne Rückfrage verarbeiten.

## Eingangsdaten vom Mock Voice Agent

| Feld | Wert |
|---|---|
| `call_id` | `demo-call-001` |
| `customer_name` | `Max Mustermann` |
| `phone` | `+491701234567` |
| `email` | `max.mustermann@example.com` |
| `service` | `Beratungsgespräch` |
| `preferred_date` | `2026-05-08` |
| `preferred_time` | `10:00` |
| `duration_minutes` | `30` |
| `notes` | `Kunde möchte eine Erstberatung.` |
| `consent` | `true` |
| `transcript` | `Kunde möchte am Freitag um 10 Uhr einen Beratungstermin.` |

## Copy-Paste-Datensatz als JSON

```json
{
  "call_id": "demo-call-001",
  "customer_name": "Max Mustermann",
  "phone": "+491701234567",
  "email": "max.mustermann@example.com",
  "service": "Beratungsgespräch",
  "preferred_date": "2026-05-08",
  "preferred_time": "10:00",
  "duration_minutes": 30,
  "notes": "Kunde möchte eine Erstberatung.",
  "consent": true,
  "transcript": "Kunde möchte am Freitag um 10 Uhr einen Beratungstermin."
}
```

## Erwartete Prüfung in n8n

| Prüfschritt | Ergebnis |
|---|---|
| Pflichtdaten vorhanden | Ja |
| Telefonnummer vorhanden | Ja |
| Einwilligung vorhanden | Ja |
| Leistung erlaubt | Ja |
| Wunschtermin frei | Ja |

## Erwartetes Ergebnis nach der n8n-Terminlogik

| Feld | Wert |
|---|---|
| `status` | `booked` |
| `requested_slot` | `2026-05-08 10:00` |
| `booked_slot` | `2026-05-08 10:00` |
| `alternative_slots` | leer |
| `errors` | leer |

## Erwarteter Antworttext für den Voice Agent

```text
Perfekt, ich habe den Termin für 2026-05-08 um 10:00 gebucht. Sie erhalten zusätzlich eine Bestätigung.
```

## Erwarteter Google-Sheet-Eintrag

Im Google Sheet sollte eine neue Zeile entstehen mit:

| Spalte | Erwarteter Wert |
|---|---|
| `call_id` | `demo-call-001` |
| `customer_name` | `Max Mustermann` |
| `phone` | `+491701234567` |
| `email` | `max.mustermann@example.com` |
| `service` | `Beratungsgespräch` |
| `preferred_date` | `2026-05-08` |
| `preferred_time` | `10:00` |
| `duration_minutes` | `30` |
| `status` | `booked` |
| `booked_slot` | `2026-05-08 10:00` |
| `alternative_slots` | leer |
| `errors` | leer |

## Erklärung für Teilnehmer

Dieser Testfall zeigt den normalen Erfolgsweg.

Der Kunde nennt alle benötigten Daten. n8n erkennt, dass der Termin frei ist, bucht den Termin und schreibt das Ergebnis in das Google Sheet.
