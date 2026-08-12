# Wedding Tracker

A free, single-file wedding planning tracker — budget, guest lists, tasks, and day-of timelines, organized per event (ceremony, reception, engagement party, shower, rehearsal dinner, etc.).

No account, no server, no subscription, no build step. It's one `index.html` file that runs entirely in your browser.

## Getting started

1. Download or clone this repo.
2. Double-click `index.html` (or open it in any browser via `File > Open`).

That's it. No `npm install`, no dev server, no internet connection required.

## How it works

- Everything is saved automatically to your browser's `localStorage`, scoped to this file. Nothing is sent to a server — your data never leaves your computer.
- Because storage is tied to the browser + file location, use the **Export backup (.json)** button on the Dashboard regularly, and especially before clearing browser data, switching browsers, or moving the file. **Import backup** restores from that file.
- Since there's no server, storage is local to one browser on one device. If you want to share progress with a partner, export/import the backup file between you.

## Features

**Dashboard** — total budget across all events, days until your next event, rolled-up spend/paid/remaining, tasks completed, and an "upcoming" view across everything.

**Events** — click "+ Add event" to create a tab for each event you're planning. Rename, set a date/time/location/vendor/status, and add notes. Delete anytime.

**Per event:**
- **Budget** — category, item, cost, amount paid, running balance, and a status dropdown (To do / In Progress / Done). Status automatically flips to *Done* once the paid amount matches the cost — or set it manually anytime.
- **Guest list & RSVP** *(optional, click to add)* — name, RSVP status, headcount, and confirmation, with live totals (coming / tentative / not coming / unconfirmed).
- **Venue tracker** *(optional, click to add)* — compare multiple venue options side by side (cost, capacity, status, contact) while you're still deciding.
- **Tasks** — a to-do list with category, due date, priority, and status.
- **Day-of timeline** *(optional, click to add)* — a run-of-show schedule (time, duration, activity, location, notes) for the actual day.
- **Invitation image** — link to an invitation image file that lives in the same folder as `index.html`; it renders as a preview.

Optional sections stay hidden until you turn them on, so simpler events don't get cluttered with tables you don't need.

## Cost

$0, forever. No hosting, no subscription — it's a static file you open locally.

## Privacy

All data lives only in your browser's local storage. There is no backend, no analytics, and no network calls. This repository ships with generic example data only — no real personal information.
