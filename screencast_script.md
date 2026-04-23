# Screencast Script — The Social Graph
## COM-480 Data Visualization · EPFL · Milestone 3
**Target duration: 2:00 (120 seconds)**
**Presenter:** Shin Urech, Joyti Goel, Ahmed Chaouachi

---

## Preparation Checklist (before recording)

- [ ] Browser open to https://thesocialgraph.vercel.app
- [ ] Viewport at 1280×800 or wider
- [ ] Default state: User dropdown on first case, no filters active, no bridge focused
- [ ] Microphone levels tested — target –12 dB peak, no clipping
- [ ] Screen recording software running (OBS / Loom / QuickTime)
- [ ] Cursor highlighting enabled

---

## Script

---

### Beat 1 — Hook [0:00 – 0:12]

**[ACTION: Page is fully loaded at default state. Scroll slowly upward so the full hero panel is visible — title, tagline, and the dataset stats strip.]**

> "You already know hundreds of people. But the people you're *about to* meet — the ones who love the same music, grew up in the same city, are in the same stage of life — they're just one mutual friend away."

**[ACTION: Pause on the hero stats strip. Let "389,639 unique friendships" and "median second-degree reach: 166" be readable on screen for ~2 seconds.]**

> "This is The Social Graph — a tool for exploring the hidden value of your second-degree network."

*Cumulative: ~12 s*

---

### Beat 2 — The Big Picture [0:12 – 0:24]

**[ACTION: Scroll down slowly past the "Reading Guide" narrative band. Three insight cards should be visible simultaneously.]**

> "The insight is simple. Your first-degree friends form a small ring. But through each of those friends, you can reach a neighborhood that's thirty times larger — and within it, people who already match who you are."

**[ACTION: Let the three cards be readable for a moment, then continue scrolling to the interactive explorer. Stop when the control panel on the left and the ring graph on the right are both visible.]**

*Cumulative: ~24 s*

---

### Beat 3 — Meet the Ego [0:24 – 0:38]

**[ACTION: Point cursor to the "Ego user" dropdown in the Control Room. Click it to show the list of five prototype cases.]**

> "Pick any user to start. Each one is a real anonymised profile from the Last.fm UK social network."

**[ACTION: Select a user with a reasonably large second-degree pool — for example User 2003 or whichever case shows "second 100+" in the ego card. The ring graph should animate into view.]**

> "Instantly the graph draws: the selected user at the center in green, their direct friends on the first ring in blue, and candidate people-you-haven't-met yet on the outer ring in orange."

**[ACTION: Hover slowly over two or three orange candidate nodes on the outer ring. Let the tooltip appear and disappear naturally.]**

*Cumulative: ~38 s*

---

### Beat 4 — The Ring Graph [0:38 – 0:52]

**[ACTION: Move the cursor to a blue bridge node on the inner ring and hover. The connecting edges to the ego and to reachable candidates should highlight. Hold for 2 seconds.]**

> "The blue nodes are your bridge friends — the people you already know who open doors to the outer ring."

**[ACTION: Click the same bridge node. The graph should focus — non-connected candidates fade, and the bridge's connections light up fully. The stage-metrics bar should update to show "active focus: bridge X".]**

> "Click a bridge to focus on just the connections they unlock. Every orange node you see now is reachable *through this one person*."

**[ACTION: Click the bridge node again to clear the focus.]**

*Cumulative: ~52 s*

---

### Beat 5 — Bridge Panel [0:52 – 1:04]

**[ACTION: Scroll the right-hand column into view — specifically the Bridge panel listing the top eight friends by bridge count. Hover over the top entry.]**

> "The Bridge panel ranks your friends by how many doors they open. More bridges means more reach — not just more connections."

**[ACTION: Click the top bridge friend in the list. The graph refocuses on that bridge. Pause for 2 seconds then click again to deselect.]**

> "Click any bridge here to filter the whole view — graph, candidate list, everything — to just those candidates."

*Cumulative: ~1:04*

---

### Beat 6 — Candidate Panel [1:04 – 1:18]

**[ACTION: Scroll to the Candidate list panel on the right. The list should show ranked orange candidates. Click on the top candidate (if not already selected).]**

> "Candidates are ranked by how many shared attributes they have with you and how many bridge friends you have in common — the more overlap, the stronger the potential connection."

**[ACTION: The Candidate Detail panel / Path Explainer should appear below or to the side — showing the bridge path and shared traits. Point cursor to the shared traits section.]**

> "Click a candidate and you see exactly why they appear: which friend bridges you, and what you have in common — same country, same age group."

**[ACTION: Move cursor to the top-N slider. Drag it from 12 down to 6, showing the list shrink. Then back to 12.]**

*Cumulative: ~1:18*

---

### Beat 7 — Filters and AND/OR Logic [1:18 – 1:33]

**[ACTION: Scroll back to the Control Room. Click the "Same country" filter checkbox — the graph and candidate list should update live.]**

> "Now filter: check 'same country' to narrow to candidates who share your origin."

**[ACTION: Also check "Same age group". The pool thins further.]**

> "Add another filter — same age group — and the pool thins to the people most similar to you."

**[ACTION: Click the "Match all checked traits" radio button in the filter mode toggle. The results update to show only candidates matching BOTH criteria.]**

> "The AND/OR toggle lets you choose: do you want candidates who match *any* filter, or *all* of them? Switch to AND — this is your tightest, most meaningful match."

**[ACTION: Uncheck both filters to reset to open pool.]**

*Cumulative: ~1:33*

---

### Beat 8 — Storyline Cards [1:33 – 1:44]

**[ACTION: Scroll down past the graph to the Storyline section — three narrative cards labelled A, B, C should be visible.]**

> "The storyline cards narrate the journey automatically. Card A introduces the ego, B names the best bridge, and C describes the top candidate — turning raw graph data into a human sentence you can actually read."

**[ACTION: Pause on the cards so the text is legible for 3 seconds.]**

*Cumulative: ~1:44*

---

### Beat 9 — Atlas [1:44 – 1:54]

**[ACTION: Scroll further down to the Atlas section. Six bar charts / distributions should be in view. Do a slow horizontal pan across them if needed.]**

> "Below the explorer, the Atlas grounds the story in data: degree distributions, second-degree reach, country and age-group breakdowns — showing that the patterns we're exploring are real and significant across the whole network."

*Cumulative: ~1:54*

---

### Beat 10 — Closing [1:54 – 2:00]

**[ACTION: Scroll back up gently to the hero, showing the full site from above. Let the ring graph animate briefly if the page reloads, or stay on the explorer view.]**

> "The Social Graph. Who connects you to people you might like — and why. Explore it at thesocialgraph.vercel.app."

*Cumulative: ~2:00*

---

## Timing Summary

| Beat | Segment | Cumulative |
|------|---------|-----------|
| 1 | Hook — hero strip, core idea | 0:12 |
| 2 | Big Picture — insight cards, scrolling into explorer | 0:24 |
| 3 | Meet the Ego — user picker, ring graph reveal | 0:38 |
| 4 | Ring Graph — bridge hover, click-to-focus | 0:52 |
| 5 | Bridge Panel — ranking, focus via panel click | 1:04 |
| 6 | Candidate Panel — ranked list, detail view, top-N slider | 1:18 |
| 7 | Filters and AND/OR toggle — live filter interaction | 1:33 |
| 8 | Storyline Cards — A/B/C narrative | 1:44 |
| 9 | Atlas — six-chart grounding | 1:54 |
| 10 | Closing — URL call to action | 2:00 |

---

## Director Notes

- **Pace:** Speak at conversational speed (~130 words/min). Beat 7 is dense — rehearse it separately.
- **Mouse movement:** Keep the cursor smooth and intentional. Highlight → pause → next action. Jerky movement reads as uncertainty.
- **Focus beats:** Beats 4 and 5 have click-to-focus interactions. Click deliberately and hold the focused state for at least 2 seconds before releasing.
- **Filter reset:** After Beat 7, reset all checkboxes before scrolling to Beat 8 so the view looks clean when the screen pans to storyline.
- **Retake strategy:** If you mis-pace, retake individual beats rather than the whole recording. Beats 1–3 are the hardest to nail cold.
- **Resolution:** Record at 1280×720 minimum. Ensure the type in the bridge and candidate panels is readable at final export size.
- **Audio:** No background noise. Do a 30-second silence test before the full take.

---

## Word Count / Pacing Check

Total spoken words (approx.): ~240
At 130 wpm that is ~110 seconds of speech.
The remaining ~10 seconds is intentional silent screen-time during action beats (graph loading, filter responses).
This keeps the script comfortable within 2:00 without feeling rushed.
