# Repair Tracker (Meadow House)

A small web app that takes a repair from its first report to completion.
Built for the Omega Digital University Hackathon, 19 October 2026 (Problem 01 · Meadow House Repairs).

Staff can report a fault, track its progress through **New → In Progress → Completed**, see a dated and timed history of every update, and keep every record after a browser refresh.

---

## How to run it

**No installation, no build step, no server.** The whole app is one file: `index.html`.

1. Download or clone this repository.
2. Open `index.html` in a web browser (double-click it). Chrome, Edge, Firefox or Safari.

That's it. Or use the live version (GitHub Pages) linked in the repository description.

### Starting from a fresh, empty state
Data is saved in the browser's `localStorage`, so it survives a refresh. To start empty for testing, either:
- open the app in a **private / incognito window**, or
- click **Clear all data** at the bottom of the page (asks for confirmation).

The app never preloads sample data.

---

## Core features (the brief)

| Brief requirement | How the app does it |
|---|---|
| Report a fault (description, reporter, Normal/Urgent) | "Report a fault" form. Home defaults to **Meadow House**. Job ID (`MH-0001`, …) and date/time are generated automatically. New jobs start as **New**. |
| Reject empty / spaces-only description or reporter | Values are trimmed before checking. Nothing is saved, a clear message appears next to the field and in a summary, typed content is kept, and focus moves to the field to fix. |
| List and open multiple jobs independently | Job list with an **Open job** button on each. Updating one job never changes another. |
| Record progress and change status | Each job page has a Status dropdown (New / In Progress / Completed) and a progress note. |
| Latest note and dated/timed status history | Shown on the job page and job card. History is **append-only**: entries are never edited or deleted. |
| Completion needs a note | Saving **Completed** with an empty or spaces-only note is blocked. Validation runs **before** any data changes, so a rejected completion leaves the saved job exactly as it was. |
| Survive a browser refresh | Every successful change is saved to `localStorage` immediately. |
| Show typed content as plain text | All user text is inserted with `textContent`, never `innerHTML`, so `<b>check gate</b>` is shown as typed and never runs as HTML. |
| Urgent shown in words | Every priority badge says "Priority: Urgent" / "Priority: Normal". Colour is never the only signal. |

## Extra features

- **Queue tracker**: counts of New / In Progress / Completed, the next job in line, and each job's position in the queue. Urgent jobs go ahead of Normal ones.
- **Estimated start**: "Estimated start: in about 2 h", from queue position and the average time past repairs took.
- **Urgency suggestion**: as you type, faults that sound urgent (gas, flooding, no heating, sparks, door won't lock…) get a suggestion with the reason and a "Mark as Urgent" button. It only suggests: the person reporting always makes the final choice.
- **Find help**: contractors matched to the fault (plumber, carpenter, electrician, glazier, roofer, handyperson), call buttons, map search near an optional postcode, real UK emergency numbers (gas 0800 111 999, power cut 105, 999), and **Book** buttons that record the booking in the job history.
- **Home map**: tag the room a fault is in; the floor plan shows open faults per room and filters the list by room.
- **Recent activity** feed across all jobs.
- **Change priority later** (logged in history), **overdue warnings** (Urgent over 1 hour, Normal over 3 days), average completion time, search, status filters, sorting, a progress stepper, live elapsed times, quick-pick fault types, optional contact phone, **print job sheet**, and **dark mode**.

## Accessibility

- Every control has a visible, programmatic label (no placeholder-only fields).
- Fully usable by keyboard, with a visible focus outline; includes a "Skip to main content" link.
- Text contrast meets WCAG AA (4.5:1) in light and dark mode; all main text is 14px or larger.
- Errors and results are announced (live regions, `role="alert"`, `aria-describedby`, focus moved to the field or result).
- Layout works at 1440 × 900 and 390 × 844 with no sideways scrolling; long notes wrap.

---

## Testing we did

We re-ran these automatically after every change, starting from empty storage each time:

1. **All 10 published functional cases (M01–M10)**, run exactly as written: **10 / 10 passed.**
2. **The measurable UI checks**: no horizontal scroll at 1440 × 900 or 390 × 844; no text under 14px; a 200-character note fully readable; confirmations visible on screen after saving (also on a phone); a full report → open → complete run using only the keyboard. **All passed.**
3. **Automated accessibility audit** (axe-core, WCAG 2.1 A/AA rules) on the main screen, error state, job page, and both themes: **0 violations.**

## Known limits

- Data lives in **one browser on one device** (`localStorage`). There is no server or shared database, as the brief asked for a simple local app. Clearing browser data deletes the jobs.
- The **Home** field is editable but starts as **Meadow House**, so reports default to the brief's home.
- Contractors and their phone numbers are **fictional demo data** (Ofcom drama ranges 01632 960xxx / 07700 900xxx). The emergency numbers are real.
- "Find near you" links open Google Maps and need an internet connection; everything else works offline.

## Built with

Plain HTML, CSS and JavaScript in a single file: no frameworks, libraries or build tools.
Planned and built with help from Claude (an AI assistant), as permitted by the hackathon rules; we tested and reviewed the behaviour ourselves.
