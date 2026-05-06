# Voice Agent Demo – Datensatz-Übersicht

Diese Dateien gehören zur n8n-Kursdemo:

**Mock Voice Agent → n8n Webhook → Datenprüfung → Terminlogik → Google Sheet**

Die Datensätze simulieren drei typische Gesprächsergebnisse eines Voice Agents:

1. Ein vollständiger Datensatz mit erfolgreicher Terminbuchung
2. Ein unvollständiger Datensatz mit fehlender Telefonnummer
3. Ein vollständiger Datensatz mit nicht verfügbarem Wunschtermin

## Dateien

| Datei | Einsatz |
|---|---|
| `01_testcase_booked_successful_booking.md` | Erfolgreiche Buchung |
| `02_testcase_missing_phone_validation_error.md` | Fehlerfall: Telefonnummer fehlt |
| `03_testcase_slot_not_available_alternatives.md` | Fehlerfall: Wunschtermin nicht frei |
| `04_reference_free_slots.md` | Referenz: freie Demo-Termine |
| `05_reference_google_sheet_schema.md` | Referenz: Google-Sheet-Spalten |

## Didaktischer Zweck

Die Teilnehmer sollen erkennen:

- Der Voice Agent wird nur simuliert.
- Der eigentliche Prozess läuft in n8n.
- n8n arbeitet mit strukturierten Daten.
- Ein Google Sheet macht das Ergebnis sichtbar.
- Unterschiedliche Eingaben führen zu unterschiedlichen Statuswerten.

## Statuswerte in der Demo

| Status | Bedeutung |
|---|---|
| `booked` | Termin wurde erfolgreich gebucht |
| `missing_information` | Es fehlen wichtige Angaben |
| `slot_not_available` | Der gewünschte Termin ist nicht frei |

## Wichtiger Hinweis

Diese Daten sind reine Demo-Daten. Sie enthalten keine echten Kundendaten und sind für Unterricht, Tests und Präsentationen gedacht.
