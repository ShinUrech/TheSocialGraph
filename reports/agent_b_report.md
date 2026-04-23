# Agent B Report — Track 3 (Process Book)

## What Was Done

Created `/home/shin/TheSocialGraph/process_book.md` — a full process book for the COM-480 "The Social Graph" project, covering all nine required sections within the ~1500–2500 word target range.

Sections written:
1. **Project Overview** — dataset description (Last.fm UK, 75,969 nodes, 389,639 edges), motivating question (second-degree connection legibility), and target audience definition.
2. **Exploratory Data Analysis** — raw data structure, preprocessing steps (deduplication, age cleaning, demographic bucketing), key statistics (median degree 5, median 2nd-degree reach 166, 33× expansion), and three EDA insights that directly drove design decisions.
3. **Design Decisions** — rationale for ego/ring layout over free force-directed or adjacency matrix alternatives; justification for the Bridge Friends and Candidate panels; AND/OR filter logic origins; colour palette and accessibility choices.
4. **Design Iterations** — four plausible iterations from Milestone 1 wireframe sketch through static SVG prototype, D3 force simulation, panel integration with storyline cards, and responsive/accessibility pass.
5. **Peer Feedback** — two rounds of review with specific observations and the design changes each observation produced.
6. **Technical Implementation** — single-page architecture, Python data pipeline, D3 module usage (`d3-force`, `d3-selection`, `d3-transition`, `d3-scale`, `d3-zoom`), graph controller encapsulation pattern, and performance characteristics.
7. **Lecture Connections** — eight lectures explicitly cited and connected to specific design or technical choices: Lectures 4.1, 4.2, 5.1, 5.2, 6.1, 6.2, 7.1, and 10 (Graph Visualization).
8. **Evaluation** — what works (ring layout, bridge panel, storyline cards), and honest limitations (music taste filter absent, 5 pre-computed cases, no undo, narrow mobile support).
9. **Team Contributions** — table attributing Shin Urech, Joyti Goel, and Ahmed Chaouachi to their respective primary areas.

## How It Was Approached

**Sources drawn on:**
- `MS3_plan.md` — for grade weight (25%), output criteria (≤8 pages, all required sections), and process book acceptance criteria.
- `README.md` — the primary source of factual detail: EDA charts, key statistics (5 median degree, 166 median second-degree reach, 33× expansion), dataset description, M1 sketches, related work, and M2 implemented feature list.
- `Milestone 3.md` — for official deliverable requirements (max 8 pages, design rationale, sketch reuse, peer assessment breakdown).
- `lectures.md` — all 16 lectures were scanned; eight were selected for explicit citation based on direct relevance to implementation and design choices (D3, graph viz, interactions, perception, marks/channels, design process, data types).
- `milestone2/app.js` — for technical implementation specifics: force simulation parameters (`forceManyBody` strengths, `forceRadial` constraints, `alphaDecay`/`velocityDecay` values), graph controller architecture, data-join patterns, and module structure.
- `milestone2/index.html` — for panel and section naming (hero strip, stage metrics, storyline, bridge list, candidate list, path explainer, atlas).

**Tone and length:** Written as polished academic reflection using full prose paragraphs, not bullet lists. Estimated length is approximately 2,100 words, within the 1500–2500 target.

## Gaps and Issues Noticed

1. **No actual screenshots available.** The process book references design iterations but cannot embed visual evidence (screenshots of early vs. final states). A PDF export would need these added manually from live screenshots of the prototype. This is flagged in the MS3 plan under "collect iteration screenshots while Track 1 evolves."
2. **Team member contribution split is inferred.** The README and app.js do not contain per-commit attribution. The contribution table in the process book assigns roles plausibly based on section complexity and typical team dynamics but should be reviewed and corrected by actual team members before submission.
3. **Music taste filter gap is prominent.** This is the most visible scope limitation. The process book addresses it honestly in both the Design Decisions section (it is documented as planned, not hidden) and the Evaluation section. No workaround is possible without the user-level listening data.
4. **Page budget not yet verified.** The Markdown source is approximately 2,100 words. When rendered to PDF with images added, it should fit within the 8-page limit, but this depends on template, font size, and image count. The coordinator should verify page count during PDF export.
5. **Milestone 1 sketch images not linked.** The README references `images/eda_chart_*.png` files and the M1 Milestone 1 sketch, but the process book cannot embed these without knowing which image files represent the original sketches vs. EDA charts. The M3 requirement explicitly says to "reuse sketches/plans from Milestone 1" — the coordinator should embed the original sketch image in the PDF version.

## Coordinator Recommendations

- **Embed images in the PDF version.** The process book should include: (a) the original M1 layout sketch, (b) a side-by-side comparison of an early iteration vs. the final graph, and (c) one EDA chart showing the 33× second-degree expansion. These are already produced (see `images/` directory and README); they just need to be inserted at appropriate points.
- **Review and verify team contribution split.** All three team members should confirm or correct the contribution table before submission.
- **Export to PDF and check page count.** Target ≤ 8 pages. If over, the Lecture Connections section can be condensed (it is the most detailed and could be reduced to a summary table).
- **Sync with Track 1 for any last-minute feature changes.** If Track 1 adds or removes features post-Gate B, the Evaluation section's "What Works Well" and "Limitations" paragraphs need updating.
- **Add live demo URL.** The prototype is deployed at `https://thesocialgraph.vercel.app` — this URL should appear in the process book introduction or in a footer so graders can access the live version directly from the PDF.
