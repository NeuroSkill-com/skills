---
name: neuroskill-calendar
description: NeuroSkill calendar integration — fetch OS calendar events (macOS EventKit, Linux iCal files, Windows Outlook/Calendar). Shows today's meetings, upcoming events, and access status. Use when the user asks about their schedule, meetings, events, appointments, or what they have coming up.
---

# NeuroSkill Calendar

Reads calendar events directly from the operating system:

| Platform | Source |
|----------|--------|
| **macOS** | Apple EventKit — all calendars synced to Calendar.app (iCloud, Google, Exchange, local) |
| **Linux** | `.ics` files from GNOME Calendar, Evolution, KOrganizer, Thunderbird Lightning, `~/Calendars/` |
| **Windows** | `.ics` files from Outlook / Windows Calendar app paths |

---

## LLM Tool Calls

```json
{"command": "calendar_events", "args": {"start_utc": 1774396800, "end_utc": 1774483200}}
{"command": "calendar_status"}
{"command": "calendar_request_permission"}
```

**Getting timestamps**: use the `date` tool first to get `now_unix`, then compute ranges:
- Today: `start_utc = now_unix - (now_unix % 86400)`, `end_utc = start_utc + 86400`
- Next 7 days: `start_utc = now_unix`, `end_utc = now_unix + 604800`
- This week (Mon–Sun): align to Monday midnight

---

## Commands

### `calendar_events`

Fetch all events overlapping `[start_utc, end_utc]`.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `start_utc` | i64 | ✓ | Range start (UTC unix seconds, inclusive) |
| `end_utc` | i64 | ✓ | Range end (UTC unix seconds, inclusive) |

**Response**:
```json
{
  "events": [
    {
      "id":          "abc123@google.com",
      "title":       "Sprint Review",
      "start_utc":   1774468800,
      "end_utc":     1774472400,
      "all_day":     false,
      "location":    "Conference Room B",
      "notes":       "Bring laptop. Demo Q2 features.",
      "calendar":    "Work",
      "status":      "confirmed",
      "recurrence":  "FREQ=WEEKLY;BYDAY=WE"
    }
  ],
  "count": 1
}
```

`status` is one of `"confirmed"`, `"tentative"`, `"cancelled"`.
`recurrence` is the raw RRULE string (not expanded — only the base instance is returned).
`all_day` events have `start_utc` at midnight UTC; `end_utc` is the next midnight.

### `calendar_status`

Check whether the app has permission to read calendars.

**Response**:
```json
{ "status": "authorized", "platform": "macos" }
```

`status` values: `"authorized"` | `"not_determined"` | `"denied"` | `"restricted"`

If `status` is `"not_determined"`, suggest running `calendar_request_permission`.
If `status` is `"denied"`, direct the user to System Settings → Privacy & Security → Calendars.

### `calendar_request_permission`

Show the macOS system permission dialog. No-op on Linux/Windows (always authorized).

**Response**:
```json
{ "granted": true, "status": "authorized" }
```

---

## Common Queries

**"What do I have today?"**
```json
{"command": "calendar_events", "args": {"start_utc": <today_start>, "end_utc": <today_end>}}
```

**"What's my next meeting?"**
Fetch events for the next few hours, sort by `start_utc`, return the first future one.

**"Am I free at 3pm?"**
Fetch events for the 1-hour window around 3pm, check if any overlap.

**"How many meetings do I have this week?"**
Fetch `[monday_00:00, sunday_23:59]`, count non-cancelled events.

**"What's on my calendar right now?"**
`start_utc = now`, `end_utc = now + 3600`; a non-empty result means something is happening.

---

## Access Troubleshooting

| `status` | Meaning | Fix |
|----------|---------|-----|
| `not_determined` | Never asked | Run `calendar_request_permission` |
| `authorized` | All good | — |
| `denied` | User declined | System Settings → Privacy & Security → Calendars → enable Skill |
| `restricted` | MDM/parental controls | Contact device administrator |

If `calendar_events` returns `{"error": "calendar_write_only_access"}`, the user chose
"Add Events Only" in the permission dialog. Run `calendar_request_permission` again to
prompt for full read access, or direct them to System Settings → Calendars.

On **Linux** and **Windows** access is always `authorized`; if `events` is empty, no `.ics` files were found in the standard locations. The user can place exported `.ics` files in `~/Calendars/`.
