# AGENTS.md

## What this is

This repo is the codebase for a **context builder** — an app that helps designers, PMs, and researchers produce a structured context repo for their XD projects without first having to learn context engineering or set up developer infrastructure.

The app's output for each user project is a four-file context repo:

- `AGENTS.md` — orchestration and discovery rules for downstream agents
- `Personas.md` — who the project serves
- `Requirements.md` — what's being asked
- `Context Building Landscape.md` — what already exists

The app's job is to make producing these files feel like drag-and-drop, not configuration.

## The problem we're solving

Designers, PMs, and researchers are getting stuck in long prompting cycles — re-explaining the user, the brief, and the constraints every turn until the context window collapses or they give up and start over. Context engineering solves this, but the current path requires git, markdown fluency, and an understanding of agent architecture that none of these users have time to acquire.

We're removing the tax. The user brings their existing artifacts (briefs, decks, PDFs, screenshots, notes). The app produces the context repo. They get to prototyping faster, with fewer restarts.

## Design center

The four personas are documented in `Personas.md`. For product decisions in this repo:

- **Design center:** Maya (XD), Jordan (PM), Sam (Research). Non-context-native. They need the tool to make producing, editing, and sharing context immediately practical — any understanding of what context engineering is follows from use, not the other way around.
- **Constraint persona:** Alex (expert). They need the tool to *not slow them down*. Their drag-and-drop demands are a floor, not a design target.

When a feature trade-off arises, optimize for the design center. Honor Alex's constraints; don't build for them.

## The app's own context (dogfood)

This repo's own context lives in:

- `Personas.md` — the four users above
- `Requirements.md` — the app's capabilities and acceptance criteria (including the markdown conversion widget for image/PDF → markdown ingestion)
- `CompetitiveLandscape.md` — survey of context-building tools, prompting frameworks, and adjacent patterns

Read these before writing code that touches user-facing flows, ingestion, or output generation. The app eats its own dog food: the architecture it produces is the architecture it uses.

## Working patterns for agents in this repo

- **Read the context files before building features.** Don't infer the user, the brief, or the competitive frame. They're documented.
- **Name which persona a change serves.** PRs and commits should reference the persona the change is for ("for Maya: reduces blank-page friction in step 2"). Silent generality hides whose problem we're actually solving.
- **Treat ingestion as load-bearing.** Drag-and-drop, paste, and image/PDF → markdown conversion are not nice-to-haves. They are the feature that distinguishes this tool from "open a text editor."
- **Preserve user artifacts.** When the user provides a brief, persona, or research finding, the app's job is to structure it — not to rewrite it. Don't flatten Sam's research into stock personas.
- **Output must be portable.** Every file the app produces should be readable and editable outside the app, in any markdown editor or agent runtime.

## Anti-patterns

- Building flows that assume git, terminal, or developer fluency
- Onboarding that explains "what context is" as a lecture rather than through doing
- Synthetic personas / requirements / competitors generated from nothing when the user has real artifacts to ingest
- Optimizing for Alex (the power user) at the expense of Maya, Jordan, or Sam
- Producing context files that only work inside this app — they must travel

## Output contract

For each user project, the app produces a directory containing:

```
project-name/
├── AGENTS.md
├── Personas.md
├── Requirements.md
└── CompetitiveLandscape.md
```

The `AGENTS.md` the app produces for users is **not this file**. It's a project-level orchestration file describing how downstream agents (Claude Code, Cursor, etc.) should consume the other three. That template is maintained separately under `/templates/output/AGENTS.md`.

## Meta

The app's premise is that **context is a first-class artifact** — produced once, reused everywhere, maintained like code. The tool exists because that premise is currently only available to users who can already build the infrastructure themselves. The job is to make it available to everyone else, without making the experts wait.