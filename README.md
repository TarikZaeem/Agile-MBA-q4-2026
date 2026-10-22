# Agile MBA™ Q4 2026 — Course Hub

A single-page, bilingual (Arabic / English) dashboard for browsing the recordings, slides, transcripts and homework of **The Job Hackers Agile MBA™ — Q4 2026** course, one card per class.

It is a personal study companion: it collects the links that live in the course's Slack workspace in one place and adds personal progress tracking, notes and a countdown to the next session.

> This is an unofficial personal project. All course materials, recordings and links belong to The Job Hackers and its instructors.

## Features

- **One card per class** (12 classes) with title, date, instructors (linked to their Slack profiles), recording, slides, transcripts and homework.
- **Arabic / English toggle** — switches all interface text and flips the page direction (RTL ↔ LTR). The choice is remembered.
- **Light / dark theme** toggle (also follows the system preference).
- **Personal progress tracking** — "Watched" and "Homework done" checkboxes per class; the progress bar reflects what *you* completed.
- **Per-class notes** — a notes box in every card, autosaved as you type.
- **Countdown to the next session** — shows the next Friday Discussion (7:30 am Pacific, DST-aware) or any upcoming class with a date set, in your local time.
- **"NEW" badge** on the latest class that has a recording.
- **Filtering and search** — All / Available / Coming soon, plus search by class title or instructor.
- **Keyboard navigation** and visible focus styles.
- **No build step, no dependencies** — plain HTML, CSS and JavaScript (fonts are loaded from Google Fonts).

## Files

| File | Purpose |
| --- | --- |
| `agile-mba-q4-2026.html` | The whole app (markup, styles, scripts). Includes an embedded fallback copy of the data. |
| `classes.json` | The course data: classes, instructors and Friday Discussion settings. |

## Getting started

Keep both files in the same folder.

**Option 1 — open the file directly**

Double-click `agile-mba-q4-2026.html`. Browsers block `fetch()` for local files, so in this mode the page uses the data embedded inside the HTML (see [Keeping the embedded data in sync](#keeping-the-embedded-data-in-sync)).

**Option 2 — serve the folder (recommended)**

```bash
python3 -m http.server 8000
# then open http://localhost:8000/agile-mba-q4-2026.html
```

In this mode the page reads `classes.json` directly, so edits to that file show up on refresh.

## Keyboard shortcuts

| Key | Action |
| --- | --- |
| `↑` / `↓` (or `K` / `J`) | Move between class cards |
| `Enter` (card focused) | Open the class recording |
| `/` | Focus the search box |
| `L` | Switch language |
| `T` | Switch light / dark theme |
| `Esc` | Leave the search box or a notes field |

## Updating the course data

Everything content-related lives in `classes.json`:

```jsonc
{
  "fridayDiscussion": { "url": "…", "timezone": "America/Los_Angeles", "time": "07:30" },
  "teachers": { "jim": { "name": "Jim Schibler", "url": "…", "color": "#12b5cb" } },
  "classes": [
    {
      "n": 3,
      "title": "Scrum Introduction Part 1: The What and Why",
      "date": "20 Oct 2026",          // display only
      "at": "2026-10-27T14:30:00Z",    // optional, ISO time; feeds the countdown
      "teachers": ["jim"],             // keys from "teachers"
      "recording": "https://…",        // presence of this field marks the class "Available"
      "files": [["slides", "https://…"], ["transcript", "https://…"]],
      "hw": [["Homework text", "https://… or null"]]
    }
  ]
}
```

Supported `files` types: `slides`, `transcript`, `chat`, `vtt`, `clip`, `mural`.

To publish a new class, add its `recording` (and optionally `files`, `date`, `hw`) to the matching entry. The class flips from "Coming soon" to "Available" and the statistics update automatically.

### Keeping the embedded data in sync

The HTML contains a copy of `classes.json` inside a `<script type="application/json" id="fallback">` tag, used when the JSON file cannot be fetched (for example when opening the file by double-click). If you rely on that mode, paste the updated JSON into that tag as well. If you always serve the folder over HTTP, `classes.json` is the only file you need to edit.

## Data and privacy

Progress, notes and language choice are stored only in your browser's `localStorage` (`amba-progress`, `amba-notes`, `amba-lang`). Nothing is sent to any server, and the data does not sync between browsers or devices. Clearing site data removes it.

## Tech notes

- Vanilla JavaScript, no framework or bundler.
- Layout uses CSS grid and logical properties, so it works in both RTL and LTR.
- Fonts: Tajawal (Arabic) and Inter (Latin), loaded from Google Fonts.
- Next-session time is computed with `Intl.DateTimeFormat` against the `America/Los_Angeles` time zone, so daylight-saving changes are handled.

## Roadmap ideas

- Arabic titles and short summaries for each class
- Export / import of progress and notes
- "Add to calendar" (`.ics`) for upcoming sessions
- Deep links to individual classes (`#class-3`)
