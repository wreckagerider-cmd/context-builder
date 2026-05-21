# Personas.md

The users of this tool sit at the edge of prototyping — close enough to want to build, far enough from context-engineering culture that the current toolchain feels like a tax. The job of the tool is to let them produce, edit, store, and share structured context for their projects — without first having to understand what context engineering is or how to set up the infrastructure for it.

**Core insight across all personas:** the bottleneck isn't ideas, it's the prompting cycle. Long re-prompts, lost threads, and start-overs are the dominant failure mode. Context — captured once, reused everywhere — is the answer. None of them currently frame it that way.

---

## Maya — XD ready to prototype

**Role:** Senior product designer (5–10 yrs). Fluent in Figma, comfortable with design systems, has dabbled in code but doesn't ship it. Has watched colleagues prototype with AI and wants in.

**Jobs to be done:**
- Turn a concept into a clickable / testable prototype without going through engineering
- Stress-test design ideas against real interactions before committing
- Move from "deck of screens" to "thing you can use" in a single afternoon

**Behaviors:**
- **Fear of prompting.** Opens the chat, stares at the cursor, deletes three drafts before sending. Worries the output will be off and the time wasted.
- **Cycle fatigue.** Has had sessions where she re-prompted 10+ times trying to get the model to remember the user, the constraints, the tone — and gave up to redo it in Figma.
- Saves prompts in a Notion doc "for next time" but never reuses them — too much friction.
- Trusts visual fidelity; distrusts code outputs she can't read.

**Constraints:**
- Not a developer. Reads code; doesn't write it.
- Time-boxed — typically 1–3 hour windows between meetings.
- Has to be able to show the output to a PM or eng partner without embarrassment.

**Not:**
- Not a prompt engineer.
- Not building production code.
- Not interested in YAML, terminals, or git unless absolutely required.

**What she'll discover through use:** Once the context files exist, she stops re-explaining. The prompting cycle shortens. The model behaves like it remembers the last meeting — because it does.

**How she'd critique the work:** "I've been working for 20 minutes and I still don't have anything to show. Why?"

---

## Jordan — PM who needs to communicate concepts

**Role:** Product manager (mid-to-senior). Writes specs, runs reviews, lives in docs and roadmaps. Prototypes occasionally to align stakeholders or de-risk a bet.

**Jobs to be done:**
- Communicate a product concept with enough fidelity that leadership/eng/design align on the same thing
- De-risk an idea before pulling design or eng time
- Translate strategy docs into something interactive

**Behaviors:**
- Writes prose specs that are dense and well-structured, then watches AI tools lose the thread three prompts in.
- Frustrated when the model forgets the constraint laid out in turn one by turn five.
- Treats the AI as a junior PM that won't remember the last meeting.
- Tries to compensate by pasting the whole spec into every prompt — then runs out of context window.

**Constraints:**
- Not a designer; visual judgment is "I know it when I see it."
- Not an engineer; can't debug code that doesn't run.
- Operates in calendar-fragmented time — needs to pick up where they left off.

**Not:**
- Not building from scratch — almost always working from existing strategy docs, research, or specs.
- Not looking to learn a new methodology — looking to make the current one work better.

**What they'll discover through use:** The context files are a structured version of what they already write in specs. Extracting them once means the AI stops losing the thread — without Jordan having to paste the whole spec into every prompt.

**How they'd critique the work:** "I told it the constraint at the start and it just ignored it. What's the point?"

---

## Sam — Researcher with insights to operationalize

**Role:** UX or design researcher. Runs interviews, synthesizes findings, briefs teams. Wants to bring research into the prototyping loop earlier — not just hand off a deck.

**Jobs to be done:**
- Translate findings into testable artifacts (concepts, stimuli, prototypes for follow-up research)
- Keep the persona/user real inside downstream design and prompting work
- Reduce the "research → design → forgot the research" decay

**Behaviors:**
- Has rich, nuanced insights about users. Painstakingly describes them in a prompt. Watches the AI flatten them into stock personas by turn three.
- Carries findings across multiple projects; resents having to re-explain context every time.
- Highly sensitive to the gap between "what users said" and "what the AI assumed they want."
- Already writes good context, but in research-shaped artifacts (synthesis decks, affinity maps) — not in agent-shaped ones.

**Constraints:**
- Not a designer or engineer.
- High bar for rigor about *who the user is* — will reject outputs that misrepresent them.
- Working from existing research artifacts that don't currently translate cleanly into context files.

**Not:**
- Not a prompt engineer.
- Not interested in synthetic personas generated from nothing — wants their real findings preserved.

**What they'll discover through use:** The Personas.md file is a home for their research — once it exists, every downstream prompt inherits the real user, not a flattened AI approximation of them.

**How they'd critique the work:** "That's not who we studied. The AI is making them up."

---

## Alex — Context-fluent power user

**Role:** Senior designer, PM, researcher, or hybrid. Already deep in context engineering. Has personal skills, prompt libraries, maybe an orchestrator. Reads release notes for AI tools the day they ship.

**Jobs to be done:**
- Drop existing context (briefs, personas, competitive maps) into the tool and start working immediately
- Skip onboarding, tutorials, and setup steps
- Compose this tool with their existing infrastructure rather than replace it

**Behaviors:**
- Resents friction. Closes tabs the moment a tool asks them to set up a GitHub repo, configure an integration, or watch a tutorial.
- Already has the artifacts the tool wants — in their own files, their own format.
- Drags and drops. Pastes. Expects the tool to meet them where they are.
- Will evangelize the tool to less-fluent colleagues *if* it doesn't waste their own time.

**Constraints:**
- Time-poor; high standards.
- Allergic to setup, configuration, and "first run" experiences.
- Will silently abandon the tool if it asks for credentials, repo connections, or account creation before showing value.

**Not:**
- Not a beginner; doesn't need explanation of what context is or why it matters.
- Not interested in opinionated workflows that override their own.

**What they need to grasp:** Nothing — they already get it. The tool needs to grasp *them*: drag-and-drop, instant ingestion, no rigamarole.

**How they'd critique the work:** "I had to click through three screens before I could paste my brief. I'm out."

---

## Cross-cutting design implications

- **Onboarding introduces context through action, not explanation.** Maya, Jordan, and Sam produce their first context file before they have a name for what they just did. Alex skips it entirely.
- **The "fear of prompting" tax is real.** The tool should make the first prompt feel safer — pre-structured context reduces the blank-page risk.
- **Restart cost has to drop to near zero.** Cycle fatigue is the failure mode across three of four personas.
- **Drag-and-drop ingestion is non-negotiable.** Alex demands it; the others benefit from it. Any flow that requires git, terminal, or config blocks adoption.
- **Markdown conversion from image/PDF is a load-bearing affordance.** Researchers carry findings as PDFs; designers carry briefs as screenshots and slide exports; PMs paste from docs. The tool must turn those into context, not ask the user to retype them. *(See Requirements.md.)*