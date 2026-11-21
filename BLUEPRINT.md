# Orbit v2 — App blueprint (21 November 2026)

## Goal
An offline-first, iPhone-friendly, private task manager for a busy actuary and father. Low-friction voice capture, distinct Work/Personal/Family/Projects tasks, obvious save confirmations, due dates, reminders, completed history, spreadsheet exports and full-data portability. GitHub Pages hosts *only static app code*, not personal tasks.

## Architecture
Pure HTML/CSS/JavaScript, no frameworks, API keys, login, servers, trackers or CDNs. GitHub Pages serves `index.html`, `manifest.webmanifest`, `sw.js`, and two icon PNGs. Service worker caches app files after first HTTPS visit; once cached, it can load offline. Task persistence is primarily IndexedDB (`orbit-v2` database, `data` object store, `tasks` key), with localStorage (`orbit-tasks-v2`) fallback. Writes are asynchronous and awaited before the UI reports success. On failed writes, old in-memory tasks stay intact and a persistent visible warning appears. v1 localStorage data (`orbit-tasks-v1`) is migrated when present on the same origin.

## User workflows
1. Capture: dictate into textarea via iPhone keyboard mic, optionally use browser SpeechRecognition, separate tasks by semicolon/newline, tap Add tasks. NLP is rules/keywords, not an LLM. Edit a task to correct category and date.
2. Manage: create/edit/delete/complete, Work/Personal/Family/Projects views, Today, Upcoming, Completed, search and sort.
3. Reminders: set date/time per task; app checks only while open/foregrounded and may show browser alerts when available; *not reliable push notifications* when the app is closed.
4. Exports: choose date window, include undated tasks optionally, export XLSX or CSV, export *all* tasks as JSON regardless of date filter. Restore JSON backups, including old v1 backups, with explicit replace confirmation.
5. Offline: install to iPhone home screen from HTTPS URL in Safari and open once online to allow cache, then use offline.

## Task schema
`{id,title,category,priority,dueDate,dueTime,reminderDate,reminderTime,notes,done,createdAt,completedAt,notified}`.
Dates `YYYY-MM-DD`, local times `HH:mm`, timestamps UTC ISO. Categories: Work, Personal, Family, Projects. Priorities High/Medium/Low.

## Limitations and security
- No cloud sync across devices, no guarantee against iOS clearing website data. Export regular backups.
- A different origin (`*.netlify.app` vs `username.github.io`) has different storage. Export JSON from old site, then import at new address.
- Browser SpeechRecognition is inconsistent on Safari; use iPhone keyboard dictation.
- No closed-app or server-triggered push notifications.
- No AI backend. Keyword classification can be wrong.
- Public GitHub repository exposes code but *does not* expose tasks, unless you manually commit your backup. Never upload personal data backups.
- GitHub Pages is a static hosting service subject to published fair-use/usage limits, not unlimited storage or backend compute.

## Next version (v3)
Task review before bulk voice-save, recurring tasks, snooze, safer reminder scheduling through opted-in push infrastructure, optional encrypted sync across iPhone/desktop, categorisation learning based on corrected tasks, export by created/completed date, archive and audit history, accessibility improvements and automated browser tests.

## Acceptance tests (manual)
- Add task, confirm success, reload: persisted.
- Add multiple dictated tasks separated by semicolon: each saved.
- Edit category/due date: persists after reload.
- Tick complete and reopen: stays complete.
- Export full JSON, import into fresh browser/profile, verify counts and task details.
- XLSX opens in Excel, displays expected header and task count.
- Turn on Airplane Mode after install/first load: reopen app, add task and reload.
- If browser storage is blocked/full, show *failure* and do not clear capture text or pretend to save.
