---
name: google-calendar-workflow
version: "1.0.0"
description: "Use this when working with Alex's Google Calendar via gws, especially for the Auckland move project. Verifies direct calendar access, uses known calendar IDs, and creates or adjusts milestone/reminder events safely."
allowed-tools: shell_execute file_read
---

# Google Calendar Workflow

## When to use

Use this when creating, verifying, or adjusting Google Calendar events with `gws`, especially for the Auckland move project.

## Procedure

1. Prefer direct calendar access by known calendar ID instead of relying on `calendarList.list`.
2. For the Auckland move project, use calendar ID `46afcc410a7e0fcb5649384a63df35dae106673232001cd7d5dfe18a96cccd5d@group.calendar.google.com`.
3. Verify access with:
   - `gws calendar calendars get --params '{"calendarId":"..."}'`
   - `gws calendar events list --params '{"calendarId":"...","maxResults":5,"singleEvents":true,"orderBy":"startTime"}'`
4. Use all-day events for major milestones:
   - `"start":{"date":"YYYY-MM-DD"}`
   - `"end":{"date":"YYYY-MM-DD"}` with end date as the following day
5. For recurring reviews, use `recurrence` with an RRULE.
6. When adjusting existing events, list events first and patch by event ID instead of creating duplicates.
7. Update the local project file `calendar/planning.md` after significant calendar changes.

## Pitfalls

- `gws calendar calendarList list` may return an empty list for the service account even when direct access works.
- `primary` resolves to the service account's own calendar, not Alex's shared move calendar.
- Quote/booking timing should be aligned with the actual late-Jan 2027 move window, not generic early milestones.

## Verification

- Confirm `accessRole` is `writer` on the target calendar.
- Confirm created/updated events return `status: confirmed`.
- Keep the local planning doc in sync with the live calendar.
