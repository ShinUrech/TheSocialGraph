# The Social Graph — Process Book

**COM-480 Data Visualization · EPFL · Spring 2026**

Team: Shin Urech (327245), Joyti Goel (325374), Ahmed Chaouachi (346447)

---

## 1. Project Overview

### Dataset and Motivating Question

The project is built on the **Last.fm UK User Graph Dataset** (Zenodo, DOI: 10.5281/zenodo.10694369), published in February 2024 by the University of Pisa and CNR. The dataset contains 75,969 anonymised user nodes and 389,639 undirected friendship edges, together with per-user demographic attributes (age, gender, country) and artist–genre tag metadata. It is research-grade, fully documented, and requires only moderate preprocessing before it can support interactive exploration.

The motivating question emerged from an everyday observation: most people think of their social network as the people they already know. The more interesting layer — the friends of their friends — is invisible by default. Through this project we asked: *Can we make second-degree connections legible, and can we reveal the hidden attributes that make a specific two-hop connection worth pursuing?*

The visualisation answers that question for a chosen user ("the ego") by showing, in a single interactive view, (1) how many people lie one hop away via bridge friends, (2) which of those bridge friends carry the most onward connections, and (3) which second-degree candidates share meaningful attributes — same country, same age group, or (as a planned future extension) similar music taste — with the ego.

### Target Audience

The primary audience is any person who is curious about the breadth of their own social network. No statistical or graph-theory background is assumed. The interface is designed to reward clicking and exploring rather than reading a manual: the core insight — "you are already close to people you haven't met" — should be apparent within the first thirty seconds of interaction.

A secondary audience is students or researchers who want a worked example of ego-network exploration in a browser-based D3.js environment.

---

## 2. Exploratory Data Analysis

### Raw Data and Preprocessing

The raw dataset arrives as four separate files. The network file is a plain edge list and loads directly into any graph library without transformation. The user demographics file requires joining on user ID and handling the roughly 40 % of records with at least one missing attribute field. Age values varied widely and included implausible entries (ages below 5 or above 100) that were removed; the remaining ages were bucketed into five groups (teens, 20s, 30s, 40s, 50+) to reduce sparsity. Country strings were normalised to remove capitalisation variants.

The artist and tag metadata files are well-structured, but they contain genre assignments at the artist level, not a per-user listening log. This distinction became important during design: computing a genuine music-taste similarity between two users would require a user-to-artist play-count table that is absent from the released dataset. This limitation was identified during the EDA phase and drove the decision to design the music-taste filter as a clearly labelled planned feature rather than as a live filter.

After deduplication of reciprocal edges and removal of self-loops, the network retains 389,639 unique undirected friendships. The connected component used in the prototype spans the great majority of users.

### Key Statistics

The degree distribution is strongly right-skewed. The **median direct-friend count is 5**, a modest first-degree neighbourhood. The **median second-degree reach is 166** — a 33× expansion in a single hop. This asymmetry is the empirical justification for the project's entire framing: the data confirms that the most interesting social territory genuinely lies one step beyond direct friends.

The demographic distributions show that the dataset skews young (peak 18–29) and heavily UK-based, which is expected given the dataset's name and provenance. This means that country-based filtering is a weaker discriminator within the prototype than age-group filtering, because most users share the same country. The EDA surfaced this before implementation began, so it was documented in the interface rather than silently ignored.

The genre tag corpus contains more than a thousand distinct tags, with "rock", "indie", and "electronic" dominating the frequency distribution. This confirms that music taste is a rich enough signal to support filtering if user-level listening data is ever incorporated.

### Insights That Shaped the Design

Three EDA findings had direct consequences for the interface design:

1. **Second-degree expansion is large and fast.** Even a user with only 5 direct friends reaches over a hundred second-degree candidates. This meant the interface needed a mechanism to focus attention — a filter and a ranking system — not just a raw list.
2. **Shared-attribute filtering reduces the candidate pool substantially.** Filtering by both country and age group typically reduced the visible pool to fewer than 30 candidates from a pool of 100–300, making individual candidates inspectable without scrolling. This validated the AND/OR filter logic before a single line of UI code was written.
3. **Bridge friends vary enormously in their outward reach.** Some friends act as genuine connectors — opening doors to large, attribute-rich candidate pools — while others are relatively isolated nodes. Surfacing the top-N bridge friends by outward reach became the "Bridge Friends" panel.

---

## 3. Design Decisions

### Why an Ego/Ring Layout?

The central interaction idiom faces a fundamental layout challenge: a typical ego node is connected to 5–15 direct friends, each of whom is connected to further candidates, producing a two-hop subgraph of 100–300 nodes. A naive force-directed graph of this size, without structure, collapses into the "giant hairball" problem identified in the Lecture 10 graph-visualisation material. Edges tangle, groups overlap, and the two-hop story becomes unreadable.

We considered three layout strategies before settling on the concentric-ring approach:

- **Free force-directed layout**: Produces natural clustering but loses the explicit hop-count narrative. Tested in an early prototype; users could not tell which nodes were bridges and which were candidates at a glance.
- **Adjacency matrix**: Excellent for neighbourhood queries but entirely unsuitable for path tracing, which is the primary task here. Eliminated in the first design review.
- **Concentric ring layout with force constraints**: Fixes bridges on the first ring and candidates on the second ring using D3 radial force constraints, while allowing the simulation to distribute nodes evenly around each ring. This preserves the hop-count story visually while letting force physics handle spacing. It maps directly onto the "radial" and "focus + context" idioms from the course.

The ring layout also enables a clean progressive-reveal animation: the ego node appears first, bridges grow in on the inner ring, and candidates expand onto the outer ring — making the "one trusted friend opens the door" narrative kinetic.

### Why Bridge and Candidate Panels?

The ego-centric graph alone is insufficient for a general audience. A first-time user looking at 12–16 unlabelled coloured dots around a central node cannot immediately understand what makes any particular candidate worth selecting. The bridge and candidate panels solve this by externalising context that is hard to read from a graph:

- The **Bridge Friends panel** ranks direct friends by their total outward reach (`bridge_count`) and highlights the selected bridge when a user hovers or clicks a node. This answers "which of my friends is the best connector?" — a question that is hard to answer from the graph alone without counting edges.
- The **Candidate Detail and Path Explainer panel** shows exactly *why* a selected candidate ranks highly: how many mutual friends they share, which attributes they have in common with the ego, and the explicit bridge path (A → B → C). This supports the "details on demand" principle from Shneiderman's information-seeking mantra.

### Filter Design: AND vs. OR Logic

An early design used only AND logic: every checked attribute had to match. User testing with peers quickly showed that this was too restrictive: activating both filters often reduced the candidate list to zero for some ego users. The OR mode ("match any checked trait") was added as an explicit toggle, with the active mode shown in the stage metrics strip at all times. This directly implements the multi-criteria faceted search pattern from Lecture 5.1 on interactions.

### Colour Palette

Node groups are encoded with three distinct hues: teal (`#146c66`) for the ego, slate blue (`#6b8fb5`) for bridge friends, and amber (`#d67b52`/`#b7853e`) for candidates. Candidates with two or more shared attributes are shown in a saturated amber to create a preattentive pop-out effect (Lecture 6.1). The background is dark, which increases contrast for the hue encoding and reduces eye strain during extended exploration. All palette choices were checked against colour-blindness simulators to ensure the hue distinctions remain legible in deuteranopia and protanopia modes.

---

## 4. Design Iterations

### Iteration 0 — Milestone 1 Sketch

The Milestone 1 submission established the conceptual model but left the visual form deliberately underspecified. The initial sketch showed a radial graph with the ego at the centre and concentric rings for friends and friends-of-friends, with coloured halos indicating shared attributes. This was a pencil-and-paper wireframe; no code existed yet.

Key question left open at this stage: how to handle the case where one bridge friend accounts for the majority of candidates? In the sketch, all bridges were drawn at equal prominence.

### Iteration 1 — Static SVG Prototype

The first coded prototype used plain SVG elements with manually computed polar coordinates — no D3 force simulation. This let the team validate the ring layout concept quickly, but it exposed a problem: when candidate count dropped below eight, the outer ring looked sparse and asymmetric. When it exceeded sixteen, nodes overlapped regardless of radius.

The fix was to add a `top-N` slider that caps the visible candidate count. This made the layout predictable and gave users a meaningful control: "how many of my best matches do I want to see?"

### Iteration 2 — D3 Force Simulation Added

The static layout was replaced with a D3 force simulation using `d3.forceRadial` to soft-constrain nodes onto their respective rings. Bridges snap to the inner ring; candidates distribute around the outer ring. The simulation's charge strength is tuned by group: the ego node repels strongly (−1200), bridges moderately (−220), and candidates weakly (−90). This asymmetry keeps the ego visually dominant and prevents bridges and candidates from swapping rings under strong charge forces.

A progressive-reveal animation was added at this stage: node `opacity` transitions from 0 to 1 with a staggered `animDelay` proportional to node index and group, giving the ego → bridges → candidates reveal sequence.

### Iteration 3 — Panel Integration and Storyline Cards

Early feedback noted that the graph was visually engaging but that first-time users did not know what to click first. Three "Storyline" cards (A / B / C) were added above the graph to guide attention: *Start with User X → Bridge through User Y → Focus on User Z*. These cards update dynamically with each state change, so the narrative always reflects the current view state.

The Bridge Friends and Candidate List panels were integrated at this stage. Hovering a bridge item in the list triggers a `previewBridge` call on the graph controller, which highlights the relevant bridge node and its outgoing edges — a cross-panel linked-highlighting interaction.

### Iteration 4 — Responsive Layout and Accessibility Pass

A responsive pass ensured the layout holds down to 780 px viewport width. The SVG graph uses a `viewBox` attribute so it scales without JavaScript resizing logic. ARIA labels were added to all interactive SVG elements and panels. The `role="img"` attribute and `aria-label` on the main SVG give screen readers a meaningful description of the graph.

---

## 5. Peer Feedback

### Round 1 — Mid-Project Review

An informal review with classmates during the Milestone 2 period surfaced three recurring observations:

1. **"I don't know what the numbers mean."** Reviewers understood the graph visually but were confused by raw counts in the stats strip (e.g., "second 166"). Adding explicit labels ("second-degree reach") and the stage-metrics strip with "direct friends / two-hop pool / visible now / active focus" resolved this.
2. **"The filter panel is hard to find."** The filter controls were initially docked in a sidebar that required scrolling. Moving the control deck above the graph, with the filter panel collapsed by default, reduced the interaction cost for first-time users.
3. **"What does clicking a candidate do?"** Some reviewers clicked a candidate node and then looked around for a response. The candidate detail panel and path explainer were made more prominent with explicit section headings ("Spotlight" and "Reasoning") and a visual connection line from the selected node to the panel.

### Round 2 — Usability Walkthrough

A second review focusing on the full page flow identified the "Atlas" section (lower-page charts) as disconnected from the interactive explorer above it. Eyebrow labels ("Atlas — Dataset patterns") and introductory paragraph text were added to contextualise each chart as supporting statistical evidence rather than a separate section.

---

## 6. Technical Implementation

### Architecture

The application is a single-page, client-side JavaScript project with no build step: one HTML file (`index.html`), one stylesheet (`styles.css`), and one application module (`app.js`). All data is embedded in a companion `data.js` file as `window.MILESTONE_DATA`, a JSON object pre-processed from the raw dataset at build time via a Python pipeline. This architecture was chosen for simplicity of deployment (Vercel static host) and for grader reproducibility: opening `index.html` locally with a live-server extension requires no server infrastructure.

### Data Pipeline

The Python preprocessing pipeline performs the following steps:
1. Load the raw edge list and user-attribute CSV files.
2. Remove self-loops and deduplicate reciprocal edges.
3. Clean demographic fields: normalise country strings, filter implausible ages, assign age-group buckets.
4. Select five diverse ego users as prototype cases (varying degree, country, and age group).
5. For each ego, compute: direct friends, second-degree candidates, mutual-friend lists, shared-attribute flags, bridge counts, and a composite score ($\text{score} = \text{mutual\_friends} \times 2 + \text{shared\_attribute\_count} \times 3 + \log(\text{degree} + 1)$).
6. Serialise the result as a single JSON object embedded in `data.js`.

The pipeline output is checked in alongside the code. The raw dataset (287 MB) is linked via the Zenodo DOI rather than committed to the repository.

### D3 Usage

The application makes use of several D3 modules:

- **`d3-force`**: the core force simulation using `forceRadial`, `forceManyBody`, `forceLink`, and `forceCollide` to produce the constrained ring layout.
- **`d3-selection`** and **`d3-transition`**: for the enter/update/exit data-join pattern that drives all graph redraws.
- **`d3-scale`** and **`d3-format`**: for the Atlas section's bar and distribution charts.
- **`d3-zoom`**: for pan-and-zoom on the main graph.

The graph controller is encapsulated in a `graphController` object returned by `renderGraph`, which exposes `previewBridge` and `clear` methods. This allows the Bridge Friends panel to drive graph highlight state without coupling the panel rendering logic to D3 internals.

### Performance

The five pre-computed ego cases keep the data load small (the embedded JSON is under 500 KB). Force simulation runs at 60 fps for graphs up to 30 nodes (1 ego + up to 15 bridges + 16 candidates). Candidate count is capped by the top-N slider, so the simulation never operates on more than ~35 nodes at once, keeping the $O(n^2)$ charge-force computation well within interactive frame budgets.

---

## 7. Lecture Connections

The design of this project drew explicitly on material from eight of the sixteen COM-480 lectures.

**Lecture 4.1 — Data** introduced the distinction between data types and data semantics. The friendship graph maps directly onto the "Network / Tree" dataset type (G(V, E, W) with undirected edges), and the demographic attributes are categorical (country, age group, gender) and ordinal (age group) variables. Understanding these type distinctions was essential for choosing appropriate visual channels — hue for group identity, position for topology — rather than encoding categorical data with a length or area channel.

**Lecture 4.2 — D3** provided the core technical foundation. The enter/update/exit pattern underpins every panel in the application; the data-join is called on `renderGraph`, `renderBridgeList`, `renderCandidateList`, and every other rendering function. `d3.forceSimulation` and the radial force in particular enabled the constrained ring layout that distinguishes this project from a free-form hairball.

**Lecture 5.1 — Interactions & Views** provided two critical design principles. Shneiderman's **Visual Information Seeking Mantra** — *overview first, zoom and filter, details on demand* — maps directly onto the page flow: the hero overview strip and storyline cards provide the overview; the filter controls and top-N slider support zoom and filter; the candidate detail and path explainer panels provide details on demand. The lecture's coverage of **faceted search** validated the AND/OR filter toggle as the correct interaction idiom for multi-attribute filtering.

**Lecture 5.2 — Interactive D3** provided the animation and transition techniques used in the progressive-reveal sequence and in the hover/focus state transitions throughout the graph. The `d3.transition().duration().ease()` pattern is used for opacity and radius changes on all interactive state changes.

**Lecture 6.1 — Perception & Color** directly influenced the palette and encoding choices. The three-hue system (teal / slate blue / amber) exploits preattentive hue pop-out to make group membership legible without cognitive effort. The saturated amber variant for high-overlap candidates applies the **preattentive pop-out** principle: nodes with two or more shared attributes visually separate from the rest of the candidate ring before the user consciously inspects them.

**Lecture 6.2 — Marks and Channels** provided the theoretical justification for encoding node group as hue (an identity channel appropriate for categorical data) and node size as a secondary indicator of focus state (a magnitude channel). The expressiveness principle — the encoding should only express what is in the data — guided the decision not to encode node degree as size, since degree is not directly relevant to the second-degree exploration task.

**Lecture 7.1 — Designing Viz** introduced Munzner's **four nested levels of visualisation design**: domain situation, data/task abstraction, visual encoding/interaction idiom, and algorithm. This framework structured the design review after Milestone 1 and helped the team identify that the original sketch had a clear domain situation and data abstraction but had not yet resolved the encoding idiom. It also reinforced the value of the **overview + detail** idiom rather than a single all-in-one view.

**Lecture 10 — Graph Visualization** is the lecture most directly relevant to the project. It covered the tradeoffs between node-link diagrams, adjacency matrices, and implicit representations; the "giant hairball" problem with unconstrained force-directed layouts; circle and radial layouts as constrained alternatives; and the concept of multivariate graphs where topology and attributes interact. The decision to use radial force constraints rather than a pure spring layout was directly motivated by this lecture's discussion of circle layout and edge bundling as clutter-reduction strategies.

---

## 8. Evaluation

### What Works Well

The ego/ring layout successfully communicates the two-hop structure to first-time users. In informal reviews, all five test users understood the bridge/candidate distinction within two minutes of first interaction without reading any documentation. The progressive-reveal animation reinforces the narrative sequence effectively.

The Bridge Friends panel achieves its goal: users can immediately identify which friends are the strongest connectors and click to focus the candidate list through a single bridge. This is a non-obvious insight that the raw graph alone does not surface.

The Storyline cards (A / B / C) provide enough narrative scaffolding that the interactive section guides itself. The design succeeds at the "details on demand" level: candidate detail and path explanation appear without any separate view change.

### Limitations

**Music-taste filtering is absent.** This is the most significant gap between the original project concept and the delivered prototype. The exciting narrative — *"find someone who loves the same music as you, lives nearby, and is reachable through one friend"* — depends on per-user listening data that is not in the released dataset. The filter slot exists in the UI, labelled clearly as planned, but it cannot be activated. This limitation is documented transparently in the interface and in the README.

**Five pre-computed ego cases.** The prototype explores only five ego users, selected to show diverse graph structures. A production version would allow any of the 75,969 users to be selected, requiring server-side graph computation or a WebAssembly graph library running in the browser. The current architecture prioritises zero-infrastructure deployment over breadth of exploration.

**No undo history.** Filter and focus changes take effect immediately and can only be reversed by manually resetting the controls. A history/undo mechanism would improve explorability for power users.

**Limited screen width support below 780 px.** The two-ring graph requires a minimum canvas size to remain legible. On narrow mobile viewports, the graph degrades to a scrollable view rather than a responsive rearrangement.

---

## 9. Team Contributions

The project was developed in parallel across overlapping tracks, with each member owning a primary area while contributing to shared reviews and integration.

| Team Member | Primary Responsibilities |
|---|---|
| Shin Urech (327245) | Project coordination, data pipeline (Python preprocessing, JSON serialisation), D3 force simulation architecture, graph controller design, responsive layout |
| Joyti Goel (325374) | EDA notebooks and statistical analysis, Bridge Friends and Candidate List panels, filter logic implementation, Storyline cards |
| Ahmed Chaouachi (346447) | Hero section and overview strip, Atlas section (six lower-page charts), colour palette design, accessibility pass (ARIA labels, keyboard navigation), CSS polish |

All team members contributed to the process book, peer review rounds, and final integration testing. Design decisions were made collectively through synchronous reviews at each milestone gate.

---

*Process book version: Milestone 3 final draft. April 2026.*
