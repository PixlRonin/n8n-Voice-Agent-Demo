# Referenz – Google-Sheet-Struktur

## Einsatz

Diese Datei beschreibt die Struktur des Google Sheets für die Voice-Agent-Demo.

Das Sheet dient als sichtbares Demo-System.

## Tabellenname

```text
voice_agent_demo_bookings
```

## Tabellenblatt

```text
bookings
```

## Spalten in Zeile 1

| Spalte | Feldname | Bedeutung |
|---|---|---|
| A | `timestamp` | Zeitpunkt des Eintrags |
| B | `call_id` | Eindeutige Nummer des simulierten Anrufs |
| C | `customer_name` | Name des Kunden |
| D | `phone` | Telefonnummer des Kunden |
| E | `email` | E-Mail-Adresse des Kunden |
| F | `service` | Gewünschte Leistung |
| G | `preferred_date` | Wunschdatum |
| H | `preferred_time` | Wunschzeit |
| I | `duration_minutes` | Dauer des Termins in Minuten |
| J | `status` | Ergebnis der Verarbeitung |
| K | `booked_slot` | Gebuchter Termin, falls erfolgreich |
| L | `alternative_slots` | Ersatztermine, falls Wunschtermin nicht frei ist |
| M | `speak` | Antworttext für den Voice Agent |
| N | `notes` | Interne Notiz zum Gespräch |
| O | `transcript` | Kurze Gesprächszusammenfassung |
| P | `errors` | Fehlende oder fehlerhafte Angaben |

## Copy-Paste-Kopfzeile für Zelle A1

Diese Zeile kann direkt in Zelle A1 eingefügt werden:

```text
timestamp	call_id	customer_name	phone	email	service	preferred_date	preferred_time	duration_minutes	status	booked_slot	alternative_slots	speak	notes	transcript	errors
```

## Statuswerte

| Status | Bedeutung |
|---|---|
| `booked` | Termin wurde erfolgreich gebucht |
| `missing_information` | Es fehlen wichtige Angaben |
| `slot_not_available` | Der gewünschte Termin ist nicht frei |


## Apps Script

Lösche alles, was dort steht, und füge diesen Code ein:

const SECRET = "DEMO_SECRET_123";

function doGet(e) {
  return jsonResponse({
    ok: true,
    message: "Voice Agent Demo Endpoint ist online."
  });
}

function doPost(e) {
  try {
    const body = JSON.parse(e.postData.contents || "{}");

    if (body.secret !== SECRET) {
      return jsonResponse({
        ok: false,
        error: "Unauthorized: Secret fehlt oder ist falsch."
      });
    }

    const ss = SpreadsheetApp.getActiveSpreadsheet();
    const sheet = ss.getSheetByName("bookings") || ss.getSheets()[0];

    const alternativeSlots = Array.isArray(body.alternative_slots)
      ? body.alternative_slots.join(" | ")
      : (body.alternative_slots || "");

    const errors = Array.isArray(body.errors)
      ? body.errors.join(" | ")
      : (body.errors || "");

    const row = [
      new Date(),
      body.call_id || "",
      body.customer_name || "",
      body.phone || "",
      body.email || "",
      body.service || "",
      body.preferred_date || "",
      body.preferred_time || "",
      body.duration_minutes || "",
      body.status || "",
      body.booked_slot || "",
      alternativeSlots,
      body.speak || "",
      body.notes || "",
      body.transcript || "",
      errors
    ];

    sheet.appendRow(row);

    return jsonResponse({
      ok: true,
      message: "written_to_google_sheet",
      status: body.status || "",
      speak: body.speak || "",
      booked_slot: body.booked_slot || "",
      alternative_slots: body.alternative_slots || [],
      errors: body.errors || []
    });

  } catch (error) {
    return jsonResponse({
      ok: false,
      error: String(error && error.message ? error.message : error)
    });
  }
}

function jsonResponse(data) {
  return ContentService
    .createTextOutput(JSON.stringify(data))
    .setMimeType(ContentService.MimeType.JSON);
}


## Didaktischer Zweck

Das Google Sheet zeigt den Teilnehmern, dass der Workflow nicht nur intern läuft.

Es entsteht ein sichtbarer Eintrag.

Dadurch wird der Automatisierungsprozess greifbar.
