# Requirements.md

## Problem

Designers, PMs, and researchers want the benefits of context engineering — fewer prompting cycles, less restart cost, more reliable AI output — without learning git, terminals, or markdown conventions. The current path requires developer infrastructure. We're collapsing that path into a drag-and-drop app.

See `Personas.md` for the four users. See `AGENTS.md` for design center and constraint personas.

## Success criteria

- A non-context-native user (Maya, Jordan, or Sam) can produce a complete four-file context repo for a new project in under 15 minutes, working from artifacts they already have.
- A context-fluent user (Alex) can ingest an existing set of artifacts and reach a working repo in under 2 minutes.
- The output files are portable: openable and editable in any markdown editor, usable in any agent runtime, no app lock-in.
- Users return for a second project. Context becomes a habit, not a one-time exercise.

## Scope

**In scope:**
- Producing and editing the four-file context repo (AGENTS, Personas, Requirements, CompetitiveLandscape) for an XD project
- Drag-and-drop ingestion of existing artifacts (markdown, PDF, image, docx)
- Wizard-based generation for users who don't have artifacts yet
- Visual project browser
- Local storage / local filesystem read-write

**Out of scope (initial release):**
- Multi-user collaboration / real-time editing
- Cloud sync or accounts
- Direct integration with Claude Code, Cursor, or other agents (the output files are the integration)
- Versioning / git integration beyond reading and writing to a folder
- Generating the actual design output (prototypes, mockups, copy) — this tool builds the *context* for that work, not the work itself

---

## Core capabilities

### 1. Start screen

The first thing a user sees. Three entry paths, visible without scrolling, no account required.

**Paths:**
- **Drag and drop** — drop files (md, pdf, png, jpg, docx) or a folder onto the canvas. The app ingests, converts, and routes content into the appropriate context files for a new project.
- **Link to folder** — point the app at an existing directory on disk. The app reads/writes context files there. Changes the user makes outside the app sync in.
- **Open existing** — open a previously created context repo (a folder containing the four files). Same as "Link to folder" but framed for users returning to prior work.
- **Start from scratch** — opens the wizard (see below) for users with no artifacts yet.

**Acceptance:**
- All four paths accessible from the start screen with no nested menus
- Drag-and-drop accepts both individual files and folders
- Linking to a folder is a single OS-native file picker action — no configuration steps
- If the user has prior projects, those appear as cubes on the start screen alongside the entry paths (see capability 5)

**Serves:** All personas. Drag-and-drop and "link to folder" are non-negotiable for Alex; "Start from scratch" and visual cues serve Maya, Jordan, Sam.

---

### 2. Blank-start wizard

For users who don't have artifacts to ingest. Builds `Personas.md` first, because the user is the load-bearing input.

**Flow:**
1. **Project name and one-sentence problem statement** — captures the ask in plain language
2. **Segmentation inputs** — who are we designing for? Role, context, organizational layer. Multi-select with free text.
3. **Jobs-to-be-done** — what is each segment trying to accomplish? Free text per segment, with prompts that scaffold without dictating.
4. **Constraints and exclusions** — what each persona is not, what they can't do, what they don't have access to
5. **Review** — the wizard shows the generated `Personas.md` in editable form. User can revise inline before committing.

**Acceptance:**
- A user with no prior context-engineering knowledge can complete a single persona in under 5 minutes
- The wizard never asks the user to write markdown directly — it produces markdown from structured input
- The wizard is skippable per persona — if Sam already has rich research notes, they should drag those in instead
- Output is a complete `Personas.md`, not a stub — at least one persona with all required fields

**Serves:** Maya, Jordan, Sam (primary). Alex bypasses this path entirely.

**Open question:** Does the wizard extend beyond personas — into Requirements and CompetitiveLandscape — or is it persona-only at v1? Recommend persona-only initially; the other two files have lower restart costs to write directly.

---

### 3. Markdown conversion widget

Ingests non-markdown artifacts and converts them to markdown ready for routing into one of the four context files.

**Capabilities:**
- **PDF → markdown** — extracts text, preserves heading hierarchy, lists, and tables where possible
- **Image → markdown** — OCR for screenshots of briefs, slides, whiteboard photos; preserves layout where structure is clear
- **Scanned PDF → markdown** — OCR fallback when text extraction returns empty
- **Docx → markdown** — direct conversion preserving headings, lists, links

**Acceptance:**
- A user can drop a 5-page PDF brief and receive usable markdown in under 30 seconds
- Conversion preserves headings, lists, and tables; flags ambiguous structure rather than silently flattening it
- After conversion, the user sees the markdown output and chooses which context file it routes into (Personas / Requirements / CompetitiveLandscape) — or splits it across files
- The app never silently rewrites user content during conversion. Cleanup is a separate, opt-in step.

**Serves:** All personas, but especially Sam (PDFs of research findings) and Maya (slide exports, screenshots).

**Open question:** Client-side conversion (privacy, no infra, faster for small files) vs. server-side (better OCR quality for scanned PDFs)? Recommend client-side default with optional server-side OCR for low-quality scans.

---

### 4. Context output

Every project produces a folder containing four markdown files.

**Output contract:**
```
project-name/
├── AGENTS.md
├── Personas.md
├── Requirements.md
└── CompetitiveLandscape.md
```

**Acceptance:**
- All four files are plain markdown, readable in any editor
- The `AGENTS.md` produced for users is a project-level orchestration file (template maintained in this repo under `/templates/output/AGENTS.md`) — not a copy of *this* repo's AGENTS.md
- The folder is the canonical source of truth. The app reads from and writes to it directly; the app is not a database with markdown as an export format.
- Files the user edits outside the app are respected and reloaded on next open
- Empty / incomplete files are valid states — the app shows completeness without blocking the user

**Serves:** All personas. Portability matters most to Alex; the user-friendly structure matters most to Maya/Jordan/Sam.

---

### 5. Project visualization — the cube grid

The home view. Each project is represented as an **isometric cube** in a grid of cubes. The cube is the visual identity of a context repo — context as an object you own, not a file you misplaced.

**Visual:**
- Each cube renders in isometric projection (three faces visible: top, front-left, front-right)
- Cubes arrange in a responsive grid
- Empty state: a single "new project" cube, ghosted, with the entry paths from capability 1

**Cube state:**
- Project name visible on or below the cube
- Last-updated timestamp
- **Completeness indicator** — visual signal for how filled-in each of the four context files is. One option: each visible face of the cube corresponds to a context file (top = AGENTS, two front faces = Personas + Requirements, fourth face implied for CompetitiveLandscape — or surface it via hover/flip). Another option: the cube fills/saturates as content grows. Decide during design.
- Hover state: brief metadata preview
- Click: opens the project

**Acceptance:**
- A user with 10 projects can scan their grid and identify which projects are recent, which are incomplete, and which one they want — in under 5 seconds
- The grid scales: 1 project, 5 projects, 50 projects all read clearly
- The visualization is functional, not decorative — completeness is information, not flourish

**Serves:** All personas — the metaphor (context as an owned object) supports the underlying premise that context is a first-class artifact.

**Open design question:** How literally does the cube map to the four files? Six faces don't divide evenly by four. Worth prototyping before specifying. Could be:
- (a) literal face-to-file mapping with two faces reserved for metadata/versioning
- (b) cube as identity, with completeness shown as fill / saturation / opacity
- (c) cube as identity with file states shown on hover only

---

### 6. Editing — agent mode and direct mode

Every context file can be edited two ways, switchable at any time:

- **Work with an agent** — describe changes in plain language; an LLM proposes edits to the relevant file(s)
- **Edit myself** — direct markdown editing in the app

The toggle is persistent and visible. Mode is per-file, not per-session — a user can edit `Personas.md` with the agent while editing `Requirements.md` directly. Both modes write to the same source-of-truth files on disk.

**Agent mode:**
- The agent has full access to all four context files in the current project
- Edits are proposed as **diffs**, not silent rewrites — the user reviews before applying
- The agent can edit one file or coordinate edits across multiple (e.g., a new persona may surface implications in `Requirements.md`)
- The agent never invents artifacts the user didn't provide. Synthetic content requires explicit user approval (per the "preserve user artifacts" principle in `AGENTS.md`).
- Conversation history persists **per project**, not globally — each project has its own working memory

**Direct mode:**
- In-app markdown editor with live preview
- Standard affordances: undo/redo, find/replace, drag-and-drop ingestion (triggers the markdown conversion widget for non-markdown drops)
- **"Open in external editor"** option — launches the user's default markdown editor and watches the file for changes (serves Alex)

**Acceptance:**
- A user can switch modes mid-session without losing in-progress work
- Agent edits are always shown as diffs before applying — never silent rewrites
- Direct edits save automatically — no explicit save button
- External-editor mode survives the user closing the app; reopening picks up the latest disk state

**Serves:** All personas. Agent mode reduces blank-page friction for Maya, Jordan, and Sam. Direct mode (especially with external editor) serves Alex. The dual-mode structure makes the same tool legible to both ends of the expertise spectrum.

**Open question:** Diff acceptance — block-by-block, all-at-once, or both? Recommend both (block-by-block for surgical edits, "accept all" for bulk).

---

## Hard constraints

- **No git.** No terminal. No config files the user has to edit by hand.
- **No account or login** for v1. Local-only.
- **Files on disk are the source of truth.** The app does not own the data.
- **Markdown output is human-readable.** No tool-specific syntax, no front-matter the user has to understand, no proprietary extensions.

## Soft preferences

- Fast cold start (open app → working in under 5 seconds)
- Keyboard-accessible for power users (Alex)
- Visible undo for all destructive operations
- Onboarding produces something useful immediately — understanding context engineering is a byproduct, not the goal

## Non-goals

- Replacing Figma, Cursor, Claude Code, or any tool that consumes context
- Generating final design output (prototypes, mockups, copy, code)
- Building a community / sharing layer / template marketplace at v1
- Auto-generating context with no user input (synthesized briefs, fake personas, fabricated competitive entries) — this violates the "preserve user artifacts" principle from AGENTS.md

## Known unknowns

- Whether the wizard should extend to Requirements and CompetitiveLandscape, or stay persona-only
- Whether markdown conversion runs client- or server-side (privacy, speed, OCR quality trade-offs)
- How literally the cube face mapping should encode file state
- Whether the app needs a "review with AI" pass after artifact ingestion, or whether ingestion-and-route is sufficient
- Multi-project workflows: do users need cross-project context reuse (e.g., a shared persona across projects)? Possibly v2.
- Conflict resolution if a file is edited externally while the in-app editor is open
- Whether the agent should be available from the cube grid (e.g., "create a new project conversationally") or scoped to within-project editing only in v1