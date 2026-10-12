# Wedding Tracker

A free, single-file wedding planning tracker — budget, guest lists, tasks, and day-of timelines, organized per event (ceremony, reception, engagement party, shower, rehearsal dinner, etc.).

No build step, no install. It's one `index.html` file that runs in your browser, with optional live sync across devices through your own free [Supabase](https://supabase.com) project.

## Getting started

1. Download or clone this repo.
2. Double-click `index.html` (or open it in any browser via `File > Open`).

That's it. No `npm install`, no dev server. On its own, the tracker works like a local app; set up [sync](#sync-across-devices-optional) if you want your data on more than one device.

## How it works

- Everything is saved automatically to your browser's `localStorage`, scoped to this file.
- If you connect sync, every change is also pushed to your Supabase project and pulled live into any other device connected to the same project. Your local copy keeps working if the connection drops.
- Use the **Export backup (.json)** button on the Dashboard regularly, especially before clearing browser data, switching browsers, or moving the file. **Import backup** restores from that file.

## Sync across devices (optional)

Sync lets you and your partner (or your phone and laptop) see the same data, updating live. It uses a Supabase project that you create and own; the free tier is plenty.

### 1. Create the project

Sign up at [supabase.com](https://supabase.com) and create a new project.

### 2. Create the table and storage bucket

In the project's **SQL Editor**, run:

```sql
-- One row holds all of the tracker's data
create table public.wedding_data (
  id text primary key,
  data jsonb not null,
  updated_at timestamptz default now()
);

alter table public.wedding_data enable row level security;

create policy "anon read"   on public.wedding_data for select using (true);
create policy "anon insert" on public.wedding_data for insert with check (true);
create policy "anon update" on public.wedding_data for update using (true);

-- Live updates between devices
alter publication supabase_realtime add table public.wedding_data;

-- Public bucket for invitation photos
insert into storage.buckets (id, name, public)
values ('invitation-images', 'invitation-images', true);

create policy "anon upload invites" on storage.objects
  for insert with check (bucket_id = 'invitation-images');
```

### 3. Connect each device

1. Go to **Project Settings → API** in Supabase and copy the **Project URL** and the **anon public** key.
2. In the tracker, open the Dashboard, find **Sync across devices**, paste both values, and click **Connect**.
3. Repeat on every device you want to keep in sync.

The first device to connect uploads its data. Devices that connect after that load the synced data, **replacing whatever was saved locally**, so export a backup first if a device has data you want to keep.

The status pill shows **Synced**, **Connecting…**, or **Sync error**, along with the last sync time. **Disconnect this device** stops syncing on that device; its local copy stays.

### Privacy note

The policies above let anyone who has your Project URL and anon key read and change your tracker data, so only share them with people you'd give edit access to. Invitation photos go in a public bucket, so anyone with an image's link can view it.

## Features

**Dashboard** — total budget across all events, days until your next event, rolled-up spend/paid/remaining, tasks completed, an "upcoming" view across everything, sync settings, and backup export/import.

**Events** — click "+ Add event" to create a tab for each event you're planning. Rename, set a date/time/location/vendor/status, and add notes. Delete anytime.

**Per event:**
- **Budget** — category, item, cost, amount paid, running balance, and a status dropdown (To do / In Progress / Done). Status automatically flips to *Done* once the paid amount matches the cost — or set it manually anytime. Click any column header (category, item, cost, paid, balance, status, due date) to sort ascending, descending, or back to the original order. Tick the checkboxes next to any items, even from different categories, to see a **Selected** total of their cost, paid, and balance. The header checkbox selects or deselects every row currently shown (so with the category filter on, it adds or removes that whole category), and **Deselect all** clears the selection. Selections stay in place when you change the filter.
- **Guest list & RSVP** *(optional, click to add)* — name, RSVP status, headcount, and confirmation, with live totals (coming / tentative / not coming / unconfirmed).
- **Venue tracker** *(optional, click to add)* — compare multiple venue options side by side (cost, capacity, status, contact) while you're still deciding.
- **Tasks** — a to-do list with category, due date, priority, and status.
- **Day-of timeline** *(optional, click to add)* — a run-of-show schedule (time, duration, activity, location, notes) for the actual day, automatically sorted by time.
- **Invitation photo** — upload an invitation image that shows on every connected device. Requires sync.

Optional sections stay hidden until you turn them on, so simpler events don't get cluttered with tables you don't need. Sections are collapsible, the layout works on phones, and long pages have a back-to-top button.

## Cost

$0. The tracker is a static file you open locally, and Supabase's free tier covers sync and photo storage.

## Privacy

Without sync, all data lives only in your browser's local storage, and there's no analytics or tracking. With sync, your data is also stored in the Supabase project you created (see [Privacy note](#privacy-note)). The page loads the Supabase library from a CDN (jsDelivr). This repository ships with generic example data only — no real personal information.
