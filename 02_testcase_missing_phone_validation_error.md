# Testfall 2 – Fehlende Telefonnummer

## Einsatz

Dieser Datensatz wird genutzt, um einen Validierungsfehler zu zeigen:

Der Kunde nennt zwar einen Wunschtermin, aber keine Telefonnummer.

## TEST_CASE

```javascript
const TEST_CASE = "missing_phone";
```

## Didaktischer Zweck

Die Teilnehmer sehen, dass n8n nicht blind bucht.

Der Workflow prüft zuerst, ob alle Pflichtdaten vorhanden sind.

## Eingangsdaten vom Mock Voice Agent

| Feld | Wert |
|---|---|
| `call_id` | `demo-call-002` |
| `customer_name` | `Sabine Beispiel` |
| `phone` | leer |
| `email` | `sabine.beispiel@example.com` |
| `service` | `Beratungsgespräch` |
| `preferred_date` | `2026-05-08` |
| `preferred_time` | `10:00` |
| `duration_minutes` | `30` |
| `notes` | `Telefonnummer wurde im Gespräch nicht genannt.` |
| `consent` | `true` |
| `transcript` | `Kundin möchte einen Termin, nennt aber keine Telefonnummer.` |

## Copy-Paste-Datensatz als JSON

```json
{
  "call_id": "demo-call-002",
  "customer_name": "Sabine Beispiel",
  "phone": "",
  "email": "sabine.beispiel@example.com",
  "service": "Beratungsgespräch",
  "preferred_date": "2026-05-08",
  "preferred_time": "10:00",
  "duration_minutes": 30,
  "notes": "Telefonnummer wurde im Gespräch nicht genannt.",
  "consent": true,
  "transcript": "Kundin möchte einen Termin, nennt aber keine Telefonnummer."
}
```

## Erwartete Prüfung in n8n

| Prüfschritt | Ergebnis |
|---|---|
| Pflichtdaten vorhanden | Nein |
| Telefonnummer vorhanden | Nein |
| Einwilligung vorhanden | Ja |
| Leistung erlaubt | Ja |
| Wunschtermin frei | Ja, aber nicht relevant |

## Erwartetes Ergebnis nach der n8n-Terminlogik

| Feld | Wert |
|---|---|
| `status` | `missing_information` |
| `requested_slot` | `2026-05-08 10:00` |
| `booked_slot` | leer |
| `alternative_slots` | leer |
| `errors` | `Telefonnummer fehlt` |

## Erwarteter Antworttext für den Voice Agent

```text
Danke. Ich kann den Termin noch nicht buchen, weil folgende Angaben fehlen oder nicht passen: Telefonnummer fehlt.
```

## Erwarteter Google-Sheet-Eintrag

Im Google Sheet sollte eine neue Zeile entstehen mit:

| Spalte | Erwarteter Wert |
|---|---|
| `call_id` | `demo-call-002` |
| `customer_name` | `Sabine Beispiel` |
| `phone` | leer |
| `email` | `sabine.beispiel@example.com` |
| `service` | `Beratungsgespräch` |
| `preferred_date` | `2026-05-08` |
| `preferred_time` | `10:00` |
| `duration_minutes` | `30` |
| `status` | `missing_information` |
| `booked_slot` | leer |
| `alternative_slots` | leer |
| `errors` | `Telefonnummer fehlt` |

## Erklärung für Teilnehmer

Dieser Testfall zeigt, warum Datenvalidierung wichtig ist.

Der Termin wäre zwar frei, aber n8n bucht trotzdem nicht, weil eine wichtige Kontaktinformation fehlt.

In einer echten Voice-Agent-Lösung würde der Voice Agent jetzt nach der fehlenden Telefonnummer fragen.
