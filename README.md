# Medical Center Booking Automation

Appointment scheduling system for a medical/dental center. Handles multi-channel intake, syncs with Google Calendar, and sends automated patient confirmations — no custom software required.

**Status:** Deployed — Centro Medico Barona, Milan

---

## Problem

A 6-practitioner medical center was managing appointments via phone, paper agenda, and a shared WhatsApp group. Scheduling conflicts were common. Staff spent ~1.5h/day on booking coordination. No-show rate: ~18%.

## Solution

An automated intake and scheduling pipeline that:
1. Captures appointment requests from web form or WhatsApp
2. Checks practitioner availability via Google Calendar
3. Books the slot and sends confirmation (email + optional SMS)
4. Sends a reminder 48h before the appointment
5. Logs everything in a Google Sheet for reporting

---

## Architecture

```
Patient
  │
  ├─── Web form (Netlify)
  └─── WhatsApp / Email
          │
          ▼
     Make.com
    (orchestration)
          │
    ┌─────┴──────┐
    │            │
Google Calendar  Google Sheets
(availability   (booking log +
 + booking)      reporting)
    │
    ▼
Email / SMS confirmation → Patient
```

---

## Stack

| Component | Tool |
|---|---|
| Frontend form | HTML + Netlify |
| Orchestration | Make.com |
| Calendar sync | Google Calendar API |
| Data logging | Google Sheets |
| Notifications | Gmail API + SMS (optional) |

---

## Features

- **Multi-practitioner support** — each doctor has a separate Google Calendar; the system routes by specialty
- **Conflict-free scheduling** — reads busy slots before confirming
- **Automated confirmations** — email sent immediately on booking
- **48h reminder** — reduces no-shows
- **Reporting sheet** — monthly appointment volume, practitioner load, no-show rate

---

## Repo Structure

```
/
├── make-blueprint/
│   └── scenario.json          # Make.com scenario export
├── frontend/
│   ├── index.html             # Booking form
│   └── style.css
├── sheets-template/
│   └── booking-log-template.xlsx
└── docs/
    ├── setup.md
    └── google-calendar-setup.md
```

---

## Setup

### Prerequisites
- Make.com account
- Google account with Calendar and Sheets access
- Netlify account (free tier)

### Steps

1. Deploy `frontend/index.html` to Netlify
2. Import `make-blueprint/scenario.json` into Make.com
3. Connect Google Calendar and Google Sheets in Make.com credentials
4. Configure practitioner calendar IDs in the scenario
5. Copy `sheets-template/booking-log-template.xlsx` to Google Drive and link in scenario

Full walkthrough in [`docs/setup.md`](./docs/setup.md).

---

## Results

- No-show rate: 18% → 9% (48h reminder)
- Staff booking coordination time: ~1.5h/day → ~20 min/day
- Zero scheduling conflicts since deployment

---

## About

Built by [ME](https://github.com/w1kicartel) / [AIgentFlow](https://AIgentflow.cloud) — AI automation for Italian SMBs.
