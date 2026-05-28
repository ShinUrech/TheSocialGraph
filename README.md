# Second-Degree Connection Explorer

**COM-480 Data Visualization — EPFL**
Ahmed Chaouachi (346447) · Shin Urech (327245) · Joyti Goel (325374)

An interactive D3.js visualization of curated second-degree introductions in the Last.fm UK friendship graph.

Built on the [Last.fm UK User Graph dataset](https://zenodo.org/records/10694369) (75,969 users · 389,639 friendships).

---

## View the site

### Option 1 — Local dev server (recommended)

```bash
python3 -m http.server 8000 --directory docs
# Then open http://127.0.0.1:8000
```

### Option 2 — Open directly

```bash
open docs/index.html        # macOS
start docs/index.html       # Windows
xdg-open docs/index.html    # Linux
```

Any modern browser works. No install step required.

### Option 3 — Deploy with GitHub Pages

1. Go to your repo on GitHub → **Settings → Pages**
2. Under *Build and deployment*:
   - Source: **Deploy from a branch**
   - Branch: `master`
   - Folder: `/docs`
3. Save → wait ~60 seconds
4. Your site is live at `https://<your-username>.github.io/<repo-name>/`

### Option 3 — Local dev server

```bash
cd docs && python3 -m http.server 8080
# Then open http://localhost:8080
```

---

## Project structure

```
TheSocialGraph/
├── docs/                  ← Live site (GitHub Pages deploys from here)
│   ├── index.html
│   ├── styles.css
│   ├── app.js             ← D3.js force-directed graph + all rendering logic
│   └── data.js            ← Dataset bundle (window.MILESTONE_DATA)
├── data/                  ← Raw dataset files
│   ├── network            ← Friendship edge list (75,969 nodes, 389,639 edges)
│   ├── UsersData_anonymized
│   ├── ArtistsMap
│   ├── Tags
│   └── ArtistTags         ← 246 MB — excluded from git; download from Zenodo
├── src/                   ← Python processing utilities
│   ├── music_data_utils.py
│   ├── second_degree_utils.py
│   └── build_music_eda_notebook.py
├── milestone1/            ← Milestone 1 submission + EDA
│   ├── Milestone_1_submission.ipynb
│   └── eda_second_degree.ipynb
├── milestone2/            ← Milestone 2 deliverables + build scripts
│   ├── project_brief.html       ← Printable two-page brief (open in browser)
│   ├── milestone2_answers.txt   ← Written answers to all M2 questions
│   ├── analysis.ipynb
│   ├── build_milestone2_assets.py ← Regenerates milestone2/data/analysis_summary.json
│   ├── sketches/                ← SVG wireframes
│   └── design_history/          ← Prototype screenshots
├── specs/
│   ├── Milestone_2.pdf
│   └── Milestone_3.pdf
└── CLAUDE.md
```

---

## What the site does

| Feature | Status |
|---|---|
| D3.js force-directed ego network (physics simulation) | Live |
| Staggered entrance animation (ego → bridges → candidates) | Live |
| Drag nodes to explore connections | Live |
| Hover tooltips on nodes | Live |
| Filter by shared country / age group / gender (AND/OR) | Live |
| Candidate ranking by score + mutual friends | Live |
| Bridge friend spotlight with path glow | Live |
| 5 animated D3 atlas charts + quality summary | Live |
| Story Mode: guided A → B → C overlay walkthrough | Live |
| Find a Match wizard: persona selector + live mini force graph + multi-path fan reveal | Live |
| Music-taste filter | Planned (needs user listening data) |

The live explorer intentionally ships three curated ego cases with ranked candidate slices. It reports each ego's full second-degree reach, but it is not a full arbitrary-user neighborhood browser.

---

## Dataset

**Last.fm UK User Graph Dataset**
Source: Zenodo — [10.5281/zenodo.10694369](https://zenodo.org/records/10694369)
Published: February 2024, University of Pisa / CNR

`ArtistTags` (246 MB) is excluded from this repo — download from Zenodo if needed.

---

## Process book

`process_book.html` at the repo root is the Milestone 3 process book (8 pages, A4 print-ready).
Open it in any browser, then use **File → Print → Save as PDF** to produce the PDF for submission.

---

## Rebuild data

```bash
# From repo root:
python3 milestone2/build_milestone2_assets.py   # writes milestone2/data/analysis_summary.json
# Then update docs/data.js manually from the JSON output
```

Requires Python 3.10+ with `pandas` and `networkx`.
`data/ArtistTags` (246 MB) must be present locally for the music-taste analysis; it is excluded from git.
