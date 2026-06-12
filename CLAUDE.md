# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

EduSlop is a self-contained, zero-dependency interactive curriculum viewer for a 33-lesson Multiagent Systems course in Ukrainian. The entire application ships as a single `index.html` file.

## Running the App

No build step required. Open `index.html` directly in a browser, or serve with any static file server:

```sh
npx serve .
# or
python -m http.server 8080
```

## Architecture

The project has two files:

- **`index.html`** — Complete application: HTML structure + ~660 lines of embedded CSS + ~290 lines of vanilla JavaScript + the full curriculum JSON (3,500+ lines) embedded as a `const CURRICULUM` array.
- **`multiagent_curriculum.json`** — Standalone copy of the same curriculum data (used independently if needed).

### Data Shape

Each lesson in `CURRICULUM` follows this shape:

```json
{
  "id": 1,
  "topic": "...",
  "outcome": "...",
  "instruction": {
    "description": "...",
    "duration_minutes": 90,
    "key_concepts": ["..."],
    "theory": [{ "title": "...", "content": "..." }],
    "examples": [{ "title": "...", "language": "python", "code": "..." }],
    "exercise": { "title": "...", "task": "...", "hints": ["..."] },
    "resources": [{ "title": "...", "type": "docs|article|video", "url": "..." }]
  }
}
```

### Key JS Functions (all in `index.html`)

- `renderSidebar()` — builds the module/lesson navigation tree
- `renderLesson(id)` — renders the full lesson view (theory, examples, exercise, resources)
- `renderBottomNav(id)` — renders prev/next buttons

State is held in two module-level variables: `activeLessonId` (number) and `completedLessons` (Set).

### CSS

Theming is entirely driven by 11 CSS custom properties defined on `:root` (indigo accent, dark sidebar, light content area). The layout is a fixed 300px sidebar + flexible main content area using CSS Grid.
