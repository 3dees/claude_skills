---
name: quick-reminders
description: >
  Set, list, and cancel reminders using natural language like "remind me to call mom tomorrow at 3pm" or a structured format like "Call mom | tomorrow 3pm". Use this skill whenever the user wants to set a reminder, create an alert, schedule a nudge, manage their reminders, or says things like "remind me", "set a reminder", "don't let me forget", "what reminders do I have", or "cancel my reminder". Also trigger when the user pastes something in the "task | time" format. This skill handles parsing times, creating scheduled tasks, and managing the reminder lifecycle.
---

# Quick Reminders

A fast, lightweight reminder system with a one-time setup that lets each user choose how they want reminders to work.

## First-Time Setup

The first time someone uses this skill, check if a preferences file exists at `~/.config/quick-reminders/preferences.json`. If it doesn't, run the setup flow before doing anything else.

The setup should ask three things using the AskUserQuestion tool:

**1. How should reminders be delivered?**
- Desktop notification (via scheduled task -- this is the default)
- Email (sends a reminder to the user's email via Gmail)
- Calendar event (creates a Google Calendar event with an alert)

**2. What input format do you prefer?**
- Natural language: "remind me to call mom tomorrow at 3pm" (recommended -- just talk naturally)
- Pipe format: `Call mom | tomorrow 3pm` (fast and structured)
- Slash format: `Call mom / tomorrow 3pm` (same idea, different separator)

**3. Do you want recurring reminders?**
- One-time only (each reminder fires once)
- Both one-time and recurring (support schedules like "every day 8am")

After the user answers, save their choices to `~/.config/quick-reminders/preferences.json`:

```json
{
  "notification_method": "desktop",
  "input_format": "natural",
  "recurring_enabled": true
}
```

Then confirm setup is complete with a quick summary like: "You're all set! You'll get desktop notifications, just tell me what to remind you about naturally, and recurring reminders are enabled. Try setting your first one!"

On all future uses, read the preferences file silently and follow the user's choices without asking again.

If the user ever says "reset my reminder settings" or "change my reminder preferences," delete the preferences file and re-run setup.

## Input Format

The input format depends on what the user chose during setup. Regardless of the chosen format, always be flexible -- if someone configured pipe format but types a natural sentence (or vice versa), still handle it gracefully. The preference just sets the primary/suggested format.

**Natural language** (default):
Just say something like "remind me to call the dentist tomorrow at 2" or "don't let me forget to submit the report on Friday at 5."

**Pipe format:**
```
<what to remember> | <when>
```

**Slash format:**
```
<what to remember> / <when>
```

**One-time examples:**
- "remind me to call mom tomorrow at 3pm"
- "don't forget to submit the report friday at 5"
- `Take medicine | 30 minutes`
- `Team sync / march 10 2pm`

**Recurring examples** (if enabled):
- "remind me to stand up and stretch every hour"
- "every friday at 4pm, remind me to do my weekly review"
- `Take vitamins | every day 8am`
- `Water plants / every monday and thursday 9am`

## How It Works

Under the hood, reminders are implemented as **scheduled tasks** using the `create_scheduled_task` / `list_scheduled_tasks` / `update_scheduled_task` tools.

### Notification Methods

The method depends on the user's preference:

**Desktop notification** (default): Create a scheduled task with a prompt that tells Claude to notify the user. This is the simplest option and requires no extra tools.

**Email**: Create a scheduled task whose prompt tells Claude to send an email to the user using the Gmail tools (gmail_create_draft + send, or similar). The email subject should be "Reminder: [task]" and the body should be brief.

**Calendar event**: Instead of a scheduled task, create a Google Calendar event at the specified time with a reminder/alert. Use the calendar tools if available.

### Creating a Reminder

1. **Read preferences** from `~/.config/quick-reminders/preferences.json`.
2. **Parse the input** into a task description and a time specification.
3. **Check the current date and time** (via `date` command) so you can calculate relative times correctly.
4. **Determine the cron expression:**
   - For recurring reminders, map directly to cron (e.g., "every day 8am" becomes `0 8 * * *`).
   - For one-time reminders, create a cron that fires at the exact date/time.
5. **Create the reminder** using the user's preferred notification method:
   - `taskId`: A short kebab-case slug derived from the task (e.g., `call-mom`, `weekly-review`). If a collision exists, append a number.
   - `description`: The reminder text exactly as the user wrote it.
   - `prompt`: Varies by notification method (see above).
   - `cronExpression`: The cron string (in local time).
6. **Confirm** with a brief, friendly message showing what was set and when it will fire. Keep it to one or two lines.

### Listing Reminders

When the user asks to see their reminders (e.g., "what reminders do I have", "show my reminders", "list reminders"):

1. Call `list_scheduled_tasks`.
2. Filter to tasks that look like reminders (ones created by this skill will have reminder-related prompts).
3. Present them in a clean, scannable format:
   - Task description
   - Schedule (human-readable, not raw cron)
   - Notification method
   - Whether it's enabled or paused
   - Next run time

### Canceling Reminders

When the user wants to cancel a reminder (e.g., "cancel the call mom reminder", "remove my weekly review reminder", "stop reminding me about vitamins"):

1. Call `list_scheduled_tasks` to find the matching task.
2. If the match is ambiguous, ask the user to clarify which one.
3. Disable the task using `update_scheduled_task` with `enabled: false`.
4. Confirm the cancellation briefly.

## Tone and Style

Keep responses short and snappy. The whole point of this skill is speed. Don't over-explain.

Good: "Done! I'll remind you to call mom tomorrow at 3pm."
Bad: "I've successfully created a scheduled reminder for you. The reminder is set to trigger at 3:00 PM tomorrow, March 4th, 2026. At that time, you'll receive a notification reminding you to call your mom. You can manage this reminder at any time by asking me to list or cancel your reminders."

If the time is ambiguous (e.g., "tuesday" but there are two possible Tuesdays), pick the nearest future one and mention which date you chose.

## Edge Cases

- **No time given**: Ask for the time. Don't guess.
- **Past time**: Let the user know and ask if they meant tomorrow or a different time.
- **Vague time** like "later" or "soon": Ask them to be more specific.
- **Multiple reminders in one message**: Handle them all. Parse each one and confirm them together.
- **Recurring reminder requested but not enabled**: Let the user know recurring isn't enabled in their preferences, and offer to update the setting.