# Agent C Report — Track 4 (Screencast Script) & Track 5 (README/Packaging)

## What Was Done

### Track 5 — README Update

**File edited:** `README.md`

**Changes made:**

1. **Expanded the `## Milestone 3` section** (previously just a heading and grade note) to include:
   - Prominent live demo link: https://thesocialgraph.vercel.app
   - A deliverables table with three rows: Live demo, Process book (`process_book.md`), and Screencast (`[Screencast — Coming Soon](#)`)
   - A "What Was Delivered" bullet list summarising all M3 features

2. **Added `## Setup / Local Development` section** after M3, before the late policy:
   - Primary method: `cd milestone2/ && python3 -m http.server 8080`
   - Alternative: open `index.html` directly (with caveat about `fetch()` restrictions)
   - A four-row file table (`index.html`, `app.js`, `data.js`, `styles.css`) with descriptions
   - Explicit note: no npm, no bundler, no environment variables required

3. **Added `## Data` section** covering:
   - Dataset name, Zenodo source link, and publication context
   - Four-file table (network, UsersData_anonymized, ArtistTags, ArtistsMap) with size notes
   - Note that `ArtistTags` (246 MB) is excluded from git and linked via Zenodo
   - Key statistics: 75,969 users, 389,639 friendships, median degree 5, median second-degree reach 166

4. **All existing M1 and M2 content left fully intact.** Only the sparse M3 section was expanded; no lines were deleted from M1/M2.

---

### Track 4 — Screencast Script

**File created:** `screencast_script.md`

**Structure:** 10 timed beats totalling exactly 2:00 (120 seconds)

| Beat | Topic | Cumulative |
|------|-------|-----------|
| 1 | Hook — hero strip, dataset stats, core idea | 0:12 |
| 2 | Big picture — insight cards, scroll into explorer | 0:24 |
| 3 | Meet the Ego — user picker dropdown, ring graph reveal | 0:38 |
| 4 | Ring Graph — bridge hover, click-to-focus interaction | 0:52 |
| 5 | Bridge Panel — bridge ranking, panel click to focus | 1:04 |
| 6 | Candidate Panel — ranked list, detail view, top-N slider | 1:18 |
| 7 | Filters and AND/OR toggle — live interaction | 1:33 |
| 8 | Storyline Cards — A/B/C narrative | 1:44 |
| 9 | Atlas — six exploratory charts | 1:54 |
| 10 | Closing — URL call to action | 2:00 |

**Coverage:** Every major feature is demoed:
- Ego selection (`#case-select` dropdown)
- Ring graph (ego / bridges / candidates, `renderGraph`)
- Bridge panel (`renderBridgeList`) with hover-preview and click-to-focus
- Candidate list and detail panel (`renderCandidateList`, `renderCandidateDetail`, `renderPathExplainer`)
- Top-N slider (`#topn-range`)
- Attribute filters (`same_country`, `same_age_group`)
- AND/OR mode toggle (`renderFilterMode`)
- Storyline cards (`renderStoryline`, cards A / B / C)
- Atlas section (`renderAtlas`)
- Hero overview strip (`renderOverviewStrip`)

**Central message threaded throughout:**
> *"Who connects you to people you might like — and why."*

The script includes a preparation checklist, director notes on pacing and cursor technique, a word-count / pacing check (~240 words at 130 wpm ≈ 110 s spoken + ~10 s visual hold), and a retake strategy for efficient recording.

---

## Issues and Gaps

- **Screencast placeholder in README:** The `[Screencast — Coming Soon](#)` link will need to be updated once the video is recorded and uploaded (YouTube, Vimeo, or GitHub release). This is intentional — the coordinator should update the link after Track 4 finishes recording.

- **Process book link:** `process_book.md` is linked in the deliverables table. The actual file `/home/shin/TheSocialGraph/process_book.md` exists in the workspace (as a placeholder/draft). The link will be correct once the PDF is exported for submission. If the process book is submitted as a PDF (`process_book.pdf`), the README link must be changed to `[process_book.pdf](process_book.pdf)`.

- **Music-taste filter:** The script deliberately does not demo the "Music taste (coming soon)" greyed-out filter checkbox to avoid drawing attention to a gap. The narration in Beat 7 focuses only on the live filters. This is the right call for a 2-minute video.

- **Atlas content:** The script says "six exploratory charts" based on the code comment `renderAtlas`. The exact chart titles were not verified against the live site. If any charts are absent or renamed, Beat 9 should be adjusted to avoid describing something not visible.

- **Hero visual / mini ring graph:** Not explicitly mentioned in the script — it plays in the background of Beat 1 without narration. This works well as visual context without adding confusion.

---

## Coordinator Recommendations

1. **Screencast recording:** Use the `screencast_script.md` timing table to structure rehearsal. Record Beats 4–7 (the interactive dense section) separately first. If total runtime exceeds 2:00, trim Beats 8–9 by ~5 s each — those are the least critical for grading.

2. **README screencast link:** Once the video is uploaded, replace `[Screencast — Coming Soon](#)` with the real URL in `README.md`.

3. **README process book link:** Confirm whether graders will receive a `.md` or `.pdf` and update the link accordingly. If PDF: change `process_book.md` → `process_book.pdf` in the deliverables table.

4. **Atlas chart count:** Before recording, quick-scroll to the Atlas section on the live site and confirm the number of charts. Adjust Beat 9 narration if needed.

5. **Gate C checklist:** README is now submission-ready for Track 5's Gate C acceptance criteria ("grader can run and understand project in < 5 minutes"). No further README changes are required unless features shift significantly.
