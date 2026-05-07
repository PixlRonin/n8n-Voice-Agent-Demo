# Referenz – Voice Booking Demo - Workflow A Voice Agent

## Einsatz

Diese Datei beschreibt die Struktur des Workflows A (Voice-Agent) für die Voice-Agent-Demo.


## NODES

Manual Trigger
Code - Demo-Anruf erzeugen
HTTP Request 


## Parameters NODES

# Manual Trigger
--

# Code - Demo-Anruf erzeugen
Mode
- Run Once for All Items
Language
- JavaScript

JavaScript (nachfolgend exakt so übernehmen - ausschließlich zu DEMO zwecken)

const TEST_CASE = "booked";

// Mögliche Werte:
// "booked"
// "missing_phone"
// "slot_not_available"

const testCases = {
  booked: {
    call_id: "demo-call-001",
    customer_name: "Max Mustermann",
    phone: "+491701234567",
    email: "max.mustermann@example.com",
    service: "Beratungsgespräch",
    preferred_date: "2026-05-08",
    preferred_time: "10:00",
    duration_minutes: 30,
    notes: "Kunde möchte eine Erstberatung.",
    consent: true,
    transcript: "Kunde möchte am Freitag um 10 Uhr einen Beratungstermin."
  },

  missing_phone: {
    call_id: "demo-call-002",
    customer_name: "Sabine Beispiel",
    phone: "",
    email: "sabine.beispiel@example.com",
    service: "Beratungsgespräch",
    preferred_date: "2026-05-08",
    preferred_time: "10:00",
    duration_minutes: 30,
    notes: "Telefonnummer wurde im Gespräch nicht genannt.",
    consent: true,
    transcript: "Kundin möchte einen Termin, nennt aber keine Telefonnummer."
  },

  slot_not_available: {
    call_id: "demo-call-003",
    customer_name: "Ali Demir",
    phone: "+491711112222",
    email: "ali.demir@example.com",
    service: "Ersttermin",
    preferred_date: "2026-05-08",
    preferred_time: "12:00",
    duration_minutes: 30,
    notes: "Kunde möchte einen Termin um 12 Uhr.",
    consent: true,
    transcript: "Kunde fragt nach einem Ersttermin am Freitag um 12 Uhr."
  }
};

return [
  {
    json: testCases[TEST_CASE]
  }
];


USE CASES: jeweils die erste Zeile im Code abändern
const TEST_CASE = "booked";
const TEST_CASE = "missing_phone";
const TEST_CASE = "slot_not_available";


# HTTP Request - An Google Sheet senden
Method
- POST
URL
- HIER KOMMT EUER LINK VOM WEBHOOK AUS WORKFLOW B REIN
"https://n8n.srv.../webhook-test..."

Authentication
- None
"Send Body" on
Body Content Type
- JSON
Specify Body
- Using JSON
JSON (nachfolgend exakt so übernehmen)
{{ $json }}

