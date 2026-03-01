---
name: calendar-blocker
description: Creates private calendar events with random durations (20-60 minutes) and no reminders. Use this skill whenever the user asks to block time on their calendar, create focus blocks, add placeholder meetings, schedule buffer time, or wants private calendar holds. Also trigger when the user says things like "block my calendar", "add some meetings", "fill my schedule", "create calendar blocks", or "hold time on my calendar". This skill handles all the randomization and privacy settings automatically. Also use this skill when the user wants to remove or clean up calendar blocks previously created by this skill.
---

# Calendar Blocker

Creates private calendar events on Google Calendar with randomized durations and no reminders. Can also bulk-remove events it previously created.

## Event Identification

All events created by this skill use a unique prefix in the title to enable bulk removal later:

- **Title format:** `[CB] Hold` (or `[CB] <custom title>` if the user provides one)
- The `[CB]` prefix is the identifier. Never omit it.
- The description should always be: `Private calendar block - created by Calendar Blocker skill`

This allows the skill to search for and delete only its own events without touching anything else on the calendar.

## Event Defaults

All events created by this skill must have:

- **Visibility**: Private
- **Start times**: Always on the hour (:00) or half hour (:30). Never use odd start times like 9:14 or 2:47.
- **Duration**: Randomly chosen from: 30, 45, or 60 minutes. Vary the durations across events.
- **Reminders**: Disabled. Use `reminders: { useDefault: false, overrides: [] }` to suppress all notifications.
- **Send updates**: Set to `"none"` so no notifications are sent.

## How to Create Events

1. **Ask the user** (if not already specified):
   - What date(s) to block
   - What time range is available (e.g., 9 AM - 5 PM)
   - How many events to create
   - Event title (default: `[CB] Hold`)
   - Which calendar to use (default: `primary`)

2. **Determine the user's timezone** by checking their Google Calendar settings or asking directly.

3. **Generate random durations** for each event from the allowed values (30, 45, or 60 minutes). Mix them up.

4. **Pick start times** that fall on :00 or :30 only. Space events with gaps between them (at least one 30-min slot between events unless the user wants them packed tighter).

5. **Schedule the events** using `gcal_create_event` with these settings for each event:

```
{
  "event": {
    "summary": "[CB] Hold",
    "description": "Private calendar block - created by Calendar Blocker skill",
    "start": { "dateTime": "<start_time>", "timeZone": "<tz>" },
    "end": { "dateTime": "<end_time>", "timeZone": "<tz>" },
    "reminders": {
      "useDefault": false,
      "overrides": []
    }
  },
  "sendUpdates": "none"
}
```

6. **After creating events, output a summary** listing each event's title, date, time, duration, and event ID so the user can easily find or delete them if needed.

## How to Bulk Remove Events

When the user asks to remove, clean up, or delete calendar blocks created by this skill:

1. **Search for events** using `gcal_list_events` with `q="[CB]"` and the relevant date range.
2. **Show the user the list** of matching events and confirm they want to delete them.
3. **Delete each event** using `gcal_delete_event` after confirmation.
4. Only delete events whose title starts with `[CB]`. Never delete other events.

## Important Notes

- Always confirm the date, time range, and number of events before creating them.
- **Max 10 events per request.** If the user asks for more, create them in batches of 10 with confirmation between each batch.
- The `visibility: "private"` field ensures other people who can see the calendar only see "Busy" rather than event details. **Note:** Google Workspace admins may still be able to see private event details. Suggest generic titles by default to avoid leaking intent.
- Never send reminders or notifications for these events.
- If the user wants recurring blocks, create them individually with varied durations rather than using recurrence rules (so each instance has a unique random length).
- Start times must always be on the hour or half hour. No exceptions.
