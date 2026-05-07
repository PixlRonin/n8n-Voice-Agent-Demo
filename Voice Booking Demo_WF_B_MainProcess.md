# Referenz – Voice Booking Demo - Workflow B Main Process

## Einsatz

Diese Datei beschreibt die Struktur des Workflows B für die Voice-Agent-Demo.


## NODES

Webhook
Code - Prüfen und Terminlogik
Code - insSecret
HTTP Request - An Google Sheet senden
Respond to Webhook 


## Parameters NODES

# Webhook
HTTP Method
- POST
Path
- voice-booking-demo
Authentication
- None
Respond
- Using 'Respond to Webhook' Node

# Code - Prüfen und Terminlogik
Mode
- Run Once for All Items
Language
- JavaScript

JavaScript (nachfolgend exakt so übernehmen - ausschließlich zu DEMO zwecken)

const input = $json.body ?? $json;

const errors = [];

if (!input.call_id) errors.push("Call-ID fehlt");
if (!input.customer_name) errors.push("Name fehlt");
if (!input.phone) errors.push("Telefonnummer fehlt");
if (!input.service) errors.push("Leistung fehlt");
if (!input.preferred_date) errors.push("Wunschdatum fehlt");
if (!input.preferred_time) errors.push("Wunschzeit fehlt");

if (input.consent !== true) {
  errors.push("Einwilligung fehlt");
}

const allowedServices = [
  "Beratungsgespräch",
  "Ersttermin",
  "Rückruf"
];

if (input.service && !allowedServices.includes(input.service)) {
  errors.push("Leistung ist nicht erlaubt");
}

const freeSlots = [
  "2026-05-08 09:00",
  "2026-05-08 10:00",
  "2026-05-08 14:30",
  "2026-05-09 11:00"
];

let status = "";
let speak = "";
let bookedSlot = "";
let alternativeSlots = [];

const requestedSlot = `${input.preferred_date || ""} ${input.preferred_time || ""}`.trim();

if (errors.length > 0) {
  status = "missing_information";

  speak = `Danke. Ich kann den Termin noch nicht buchen, weil folgende Angaben fehlen oder nicht passen: ${errors.join(", ")}.`;

} else if (freeSlots.includes(requestedSlot)) {
  status = "booked";
  bookedSlot = requestedSlot;

  speak = `Perfekt, ich habe den Termin für ${input.preferred_date} um ${input.preferred_time} gebucht. Sie erhalten zusätzlich eine Bestätigung.`;

} else {
  status = "slot_not_available";
  alternativeSlots = freeSlots.slice(0, 3);

  speak = `Der gewünschte Termin ist leider nicht frei. Ich kann alternativ folgende Termine anbieten: ${alternativeSlots.join(", ")}.`;
}

return [
  {
    json: {
      call_id: input.call_id || "",
      customer_name: input.customer_name || "",
      phone: input.phone || "",
      email: input.email || "",
      service: input.service || "",
      preferred_date: input.preferred_date || "",
      preferred_time: input.preferred_time || "",
      duration_minutes: input.duration_minutes || 30,
      notes: input.notes || "",
      transcript: input.transcript || "",
      consent: input.consent === true,
      status,
      requested_slot: requestedSlot,
      booked_slot: bookedSlot,
      alternative_slots: alternativeSlots,
      speak,
      errors,
      processed_at: new Date().toISOString()
    }
  }
];


# Code - insSecret
Mode
- Run Once for All Items
Language
- JavaScript

JavaScript (nachfolgend exakt so übernehmen)

return items.map(item => {
  item.json.secret = "DEMO_SECRET_123";
  return item;
});


# HTTP Request - An Google Sheet senden
Method
- POST
URL
- HIER KOMMT EUER LINK VON GOOGLE APPS SCRIPT - DER LINK MIT EXEC AM ENDE
Authentication
- None
"Send Body" on
Body Content Type
- JSON
Specify Body
- Using JSON
JSON (nachfolgend exakt so übernehmen)
{{ $json }}

# Respond to Webhook
Respond With
- JSON
Response Body (nachfolgend exakt so übernehmen)

{
  "status": "={{ $json.status }}",
  "speak": "={{ $json.speak }}",
  "google_sheet_status": "={{ $json.message }}",
  "booked_slot": "={{ $json.booked_slot }}",
  "alternative_slots": "={{ $json.alternative_slots }}",
  "errors": "={{ $json.errors }}"
}
