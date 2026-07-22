# From Spec to Ship — v3
## A Session-Based Native AI Engineering Syllabus

> Karpathy's red-team benchmark, built as three projects of increasing complexity, with explicit learning + building scaffolding at every session.

You are not learning to code. You are learning to **direct, verify, and harden** agentic engineering at an engineer's quality bar, building on what you already do well (specs, taste, judgment).

The plan: 34 sessions (4-5 hrs each, expandable to 8) across 7 weeks (July 13 to August 30, 2026). Three projects in sequence, plus a fourth project spec'd at the end. Each project runs the full loop. Each session has its own scaffolding so building and learning happen together, not separately.

### What changed from v2 to v3

This version merges five decisions made after v2 was written. If you know v2, read only these; everything else is unchanged.

1. **Evals added to Project 3.** Session 3.5 reframed, a new golden-dataset session inserted, Session 3.9 reframed as the eval suite. Evals are the highest-signal screen for both FDE and AI PM roles and v2 produced none. Written out in full in §9. A new Eval Sanity block was added to the Oversight Checklist (§5), labeled Project 3 only.
2. **Codex integrated as a second tool.** Claude Code stays primary driver for all three projects. Codex enters at three comparison points: comparative re-implementation at 1.8, second reviewer from 2.1 on, mixed attacker panel at 3.11. Rationale and rules in §5b.
3. **Project 2 narrowed and the calendar compressed.** Project 2 runs the v2 narrowed scope (Companies, Contacts, Notes, Gmail read-only) at 8 sessions, not 10. Week 0 compresses to 3 sessions since 0.0 is done. Total is 34 sessions over 7 weeks.
4. **Public repos and a journey repo.** All repos public from first commit. A fifth "journey" repo holds syllabus-level docs and is the single portfolio URL. Created in Session 0.1-lite.
5. **Project 4 added as a lane.** The travel influencer agent is Project 4: spec'd in consolidation week using the finished playbook, built in September. Not a syllabus project; the proof that the playbook transfers.

The 7-week calendar lives in §12, The Roadmap. Everything else in this document is the v2 core, preserved because process does not go stale.

---

## Table of Contents

1. [The North Star](#1-the-north-star)
2. [The Three-Project Staircase](#2-the-three-project-staircase)
3. [The Dual-Loop Model](#3-the-dual-loop-model)
4. [The Session Template](#4-the-session-template)
5. [The Agent Oversight Checklist](#5-the-agent-oversight-checklist)
6. [Week 0: Foundation (fully fleshed)](#6-week-0-foundation)
7. [Project 1: Thesis Tracker (session outline)](#7-project-1-thesis-tracker)
8. [Project 2: Deal-Flow CRM (session outline)](#8-project-2-deal-flow-crm)
9. [Project 3: Research Triage + Red Team (session outline)](#9-project-3-research-triage--red-team)
10. [Final Consolidation](#10-final-consolidation)
11. [The RL Investment Map](#11-the-rl-investment-map)
12. [The Roadmap: July 13 to August 30](#12-the-roadmap)
13. [Appendix A: Artifact Templates (PRD, ADR, Tech Design)](#appendix-a-artifact-templates)
14. [Appendix B: Tool Stack & Setup](#appendix-b-tool-stack--setup)

Note: §5b (Codex, the second tool) sits after §5. Project 4 (§9b) sits after §9.

---

## 1. The North Star

Karpathy's "native AI engineer" hiring test: build a real project. Make it good. Make it secure. Deploy it. Then let 10 adversarial agents try to break it. They fail.

You pass when:
1. Three deployed, working Parchmount-relevant tools live on the internet
2. The third one survives 10 adversarial Claude Code agents attempting account takeover, cross-tenant data leakage, prompt injection, and destructive actions
3. You can open any file in any repo and explain why it's there
4. You have a personal playbook (CLAUDE.md template, oversight checklist, session debriefs) you can apply to project 4, 5, 6…

The thing that's actually being trained is your ability to know **which parts of software work belong to agents and which belong to you**.

---

## 2. The Three-Project Staircase

Each project teaches a new layer while reinforcing the loop. Skill carries forward (auth, tenancy, deploy, UI, CLAUDE.md, session debriefs).

| # | Project | Sessions | New layer this project introduces | Why this order |
|---|---|---|---|---|
| 1 | **Thesis Tracker** | ~8 | The full loop end-to-end at minimum complexity: spec → build → ship → light red-team | Lightest scope. You learn the loop without LLM/integration noise. |
| 2 | **Deal-Flow CRM** | ~10 | External integrations (Gmail, Calendar), more complex data model, more trust boundaries | Now you know the loop. Stretch it with real-world plumbing. |
| 3 | **Research Triage Platform** | ~12 | LLM pipeline, file uploads, async jobs, prompt injection defense, full adversarial red-team | Karpathy test target. Carries scaffolding from 1 + 2. |

What carries forward each time:
- **Project 1 → 2:** auth scaffolding, UI component library, deploy pattern, CLAUDE.md template, test patterns
- **Project 2 → 3:** all of above + integration patterns + scoring logic + multi-tenant patterns hardened

What's deliberately new each project:
- **Project 1:** the loop itself
- **Project 2:** OAuth flows to third parties, webhook handling, sync vs async, integration security
- **Project 3:** LLM API calls, prompt injection, rate-limiting expensive operations, file upload sandboxing, async job processing, **the full adversarial red-team campaign**

---

## 3. The Dual-Loop Model

You will use Claude.ai and Claude Code together. They are not interchangeable.

**Claude.ai = The Think Tank**
Open-ended exploration. Spec drafting (PRDs, tech designs, ADRs). Architectural decisions before you commit. Code review of diffs ("paste a diff, what's wrong with this?"). Unsticking when Claude Code is going in circles. Concept learning. Writing test cases as English specs before they become code.

**Claude Code = The Workshop**
Touching files, running commands, iterating in your repo. Multi-file refactors. Running tests and reading output. Git operations. Parallel work via git worktrees. The adversarial agents in Project 3.

**The Standard Loop:**
```
1. Question / new feature       → Claude.ai     → one-page brief
2. Brief                        → Claude.ai     → PRD (or feature spec)
3. PRD                          → Claude.ai     → tech design / ADR if needed
4. Tech design                  → save to repo  → docs/design/<feature>.md
5. Tech design + CLAUDE.md      → Claude Code   → task breakdown
6. Each task                    → Claude Code   → implement (tests first when possible)
7. Diff                         → Claude.ai     → review pass
8. Issues                       → Claude Code   → fix
9. Merge → commit → repeat
```

The discipline: **Claude Code does not start implementing until step 4 is done.** This is where you stop the bloat, brittle abstractions, and MenuGen-class identity bugs.

---

## 4. The Session Template

Every working session — including Week 0 sessions and every project session — follows this structure. Print it. Or paste it into your session notes. Or make a markdown template in your repo.

```markdown
# Session [N.M]: [Name]
Date: ___
Time budget: 4-5 hrs (extend to 8 if crushing it)

## 1. Objective (1 sentence)
What ships by end of session.

## 2. Prerequisite check (5 min)
- [ ] Last session debriefed? Notes saved?
- [ ] Repo clean? Working tree status checked?
- [ ] Tools running? (Claude Code launches, dev server runs)
- [ ] Any blockers from last time resolved?

## 3. Concept of the session (15-30 min, Claude.ai)
The foundation idea you're learning *because* you're hitting it today.
Examples: "What is row-level security?" / "How does OAuth actually work?" / "What is prompt injection?"
- Read: Claude.ai explainer + one or two referenced links
- Goal: be able to explain it back in 3 sentences before building

## 4. Pre-work (15-30 min, Claude.ai)
- Restate the objective in your own words
- Draft / refine the spec for this session's work
- Identify the 2-3 oversight catches you expect to make
- Write the "definition of done" checklist for this session

## 5. Build (2.5-3 hrs, Claude Code)
The actual work. Follow the dual-loop discipline.
- Paste the session spec into Claude Code
- Implement one task at a time
- Run tests after each meaningful change
- If stuck >15 min, escalate to Claude.ai for an unstick conversation

## 6. Verification (20-30 min)
- [ ] Definition of done checklist passes
- [ ] Tests pass (and there are tests for the new behavior)
- [ ] Manually exercise the new feature
- [ ] Run §5 Oversight Checklist (the relevant rows for what changed)

## 7. Review pass (15-30 min, Claude.ai)
- Paste the diff into Claude.ai
- Prompt: "Review this diff as a senior engineer. What's bloated? What's brittle? What's missing? What's the bypass for any defense added?"
- Capture findings; decide which to fix now vs note for later

## 8. Debrief (10-15 min, your notebook)
- What shipped: ___
- What broke / what was confusing: ___
- What did Claude Code do brilliantly (heavily-RL'd capability): ___
- What did Claude Code do badly (RL gap): ___
- One oversight catch I'm proud of: ___
- One oversight I missed (caught in review): ___
- Next session: ___
```

### The two files: `debriefs.md` (per-project, manual) vs `lessons.md` (cross-project, synthesized)

Two distinct files. Don't conflate them.

**`docs/debriefs.md`** — lives in each project repo. One entry per session, appended in order. **Claude Code can scaffold the entry** (date, session number, blank prompts pre-filled from the template) at the start of each session — just ask it: *"Append a new debrief entry for Session X.Y to docs/debriefs.md using the template. Leave the answers blank for me to fill in."* But the **reflection itself is yours, written manually.** Pre-filled scaffolding, manual answers. If you let the agent write your reflection, you lose 100% of the value of this file.

**`lessons.md`** — lives at the syllabus level (not per-project). One file across all three projects. You write this at Final Consolidation, with Claude.ai's help, by reading back through all your `debriefs.md` files and distilling the patterns. This is the cross-project synthesis: "across 30 sessions, here's what I learned about agent strengths, agent failure modes, my own taste signature, and what carries forward."

The debrief is the most important part of every session. Over 30+ sessions, the patterns in your debrief notes *are* your transferable skill set. They'll show you:
- Which agent capabilities you can trust
- Which agent failure modes you keep needing to catch
- What your own taste signature is

Re-read your project's `debriefs.md` before each new session.

---

## 5. The Agent Oversight Checklist

Use this on every meaningful diff. Track which catches you make and which slip past — that's your learning signal.

### Design Sanity (before code is written)
- [ ] Primary key for every entity is stable, opaque, not derived from anything mutable (no emails, no names, no slugs)
- [ ] Identity model: one user = one record where? How are providers linked back?
- [ ] Tenancy model: can user A ever read user B's data? Where is that enforced (schema + middleware)?
- [ ] Failure mode of each external call defined (timeouts, retries, circuit-breakers)
- [ ] Cost model: can a bad actor run up your bill?

### Code Hygiene (during review)
- [ ] Any file > ~300 lines? Why?
- [ ] Any function > ~50 lines? Why?
- [ ] Repeated code blocks (copy-paste smell)
- [ ] Abstractions used exactly once (premature)
- [ ] Comments that say *what* instead of *why*
- [ ] Imports you can't explain
- [ ] Untyped function signatures (in TS); untested branches

### Security Smoke Tests (before each deploy)
- [ ] Where does user input enter, and where is it interpreted as code/query/HTML?
- [ ] Any place trusting the client (e.g., "user ID comes from request body")?
- [ ] Auth required by default; routes opt-out, never opt-in
- [ ] Secrets in env vars, not in code
- [ ] Rate limiting on anything calling LLM, sending email, or expensive
- [ ] File uploads size-capped, type-validated, stored outside web root
- [ ] LLM prompts: user input quoted/delimited, not concatenated raw
- [ ] What happens if every input field is fuzzed with garbage / SQL / scripts / huge payloads?

### Eval Sanity (Project 3 only)
Do NOT run this block in Project 1 or 2. There is no LLM output to evaluate before Project 3. Running eval checks on CRUD is scope creep.
- [ ] Is there a golden dataset of inputs with known-good expected outputs?
- [ ] Is there a grader (exact match, rubric, or LLM-as-judge) for each eval?
- [ ] Does the suite detect drift when the prompt or model changes?
- [ ] Are failure modes logged and regression-tested, not just fixed once?

---

## 5b. Codex: The Second Tool

Multi-tool fluency is a market requirement for FDE and AI PM roles, and a single-vendor portfolio reads as locked-in. Codex enters the syllabus as a deliberate addition. The design principle protects your repeatability: **one primary driver per project, and it is Claude Code for all three.** Codex never touches a greenfield task during the syllabus. It re-implements shipped work, reviews diffs, and attacks.

This works because the tools converged. Codex uses AGENTS.md where Claude Code uses CLAUDE.md. Codex skills follow the same open agent skills standard (SKILL.md). Both support MCP and subagents. The syllabus teaches one discipline on one tool, then proves it transfers. That proof is the portfolio claim.

**Three entry points:**

- **Entry 1 — Session 1.8, comparative re-implementation.** Install Codex (pre-work, before the session). Translate the thesis-tracker CLAUDE.md into AGENTS.md, noting what maps and what doesn't. Re-implement one already-shipped Project 1 feature on a disposable branch using Codex against the same spec. Comparative debrief: what did each agent do differently against identical ground truth? The branch is disposable; do not merge or polish it. The artifact is the debrief.
- **Entry 2 — from Session 2.1, second reviewer.** The Standard Loop's step 7 gains a second reviewer. Claude.ai runs the senior-engineer pass; Codex `/review` runs the same diff as a different-model second opinion (read-only, no working-tree changes). Log every disagreement in the debrief; act only on findings your own oversight checklist independently flags.
- **Entry 3 — Session 3.11, mixed attacker panel.** The red-team splits adversarial agents between Claude Code subagents and Codex subagents. Different model families attack differently. The red-team log records which vendor's agent found each break. "Survived a mixed-vendor adversarial panel" is a stronger Karpathy claim than single-vendor.

**Access:** Codex is included with ChatGPT Plus and above, or pay-per-token via OpenAI API key. Decide before Week 3, record it in the journey repo docs, same secrets rules as the Anthropic key.

**Verification note:** Codex releases frequently. Re-verify the install command and auth flow against developers.openai.com/codex the week you run Entry 1, not from this document.

---

## 6. Week 0: Foundation
Three sessions (0.0 is already complete). Goal: by end of Week 0, your workspace works, the journey repo exists, you've practiced the dual-loop on a throwaway, and you have refreshed reference points for PRD / ADR / tech design.

**Session 0.0 is done.** Machine setup, accounts, folder structure, smoke test all passed. Week 0 now begins at 0.1-lite.

### Session 0.1-lite — Loop, Plan Mode, and the Journey Repo (4-5 hrs)

**Objective:** By end of session: a hello-world repo committed and pushed via the full dual-loop, Plan Mode learned, and the journey repo created as your portfolio front door.

**Concept of the session (one concept):** Plan Mode. Claude Code can explore a repo and propose a task breakdown *without touching files*. This is how you enforce "no implementation until the tech design is done" at the tool level instead of by willpower. It maps to discipline you already have. Learn only this Claude Code primitive now; Skills come at 1.8, Subagents at 3.11.

**Pre-work (Claude.ai):** Ask for a plain-English primer on terminal basics, what git is conceptually, what a code editor does, and what Plan Mode changes about how you direct Claude Code. One page.

**Build:**
1. In `~/code/ai-engineering/`, create `playground` repo (if not already), open in Cursor, launch Claude Code.
2. Use Plan Mode: ask Claude Code to *propose* (not build) a hello-world Next.js page. Read the plan. Then approve and let it build, commit, push.
3. **The journey repo, 45 min.** Create a fifth repo, `ai-engineering-journey`, public, at `~/code/ai-engineering/`. It holds: this syllabus, and empty stubs for `lessons.md`, `projects-backlog.md` (seed with the five parked ideas below), and `prompts.md`. README links the four project repos as they go live. This is the single URL you hand a client or screener.
4. **Optional global CLAUDE.md**, 5 lines or fewer, at `~/.claude/CLAUDE.md` — personal conventions only. Per-repo CLAUDE.md files carry everything project-specific. Keep the global one tiny; it pollutes every session's context.

**projects-backlog.md seed entries (parked, not for this syllabus):**
- Growth agent + thought-capture system → capstone v2 (generalize Project 4 beyond travel)
- Institutional investment research analyst → Project 5, first post-syllabus build
- Corporate CPG procurement agent → waits for a paying client to define workflows
- AI-native services infrastructure → this is the playbook, not a separate build
- (travel influencer agent is Project 4, already lane'd — not backlog)

**Verification:**
- [ ] hello-world committed and pushed; you used Plan Mode before it wrote code
- [ ] journey repo public, README links planned project repos, stubs committed
- [ ] you can explain what Plan Mode changed about the loop

**Debrief prompts:** standard set, plus: did Plan Mode feel like friction or like the discipline you already wanted? Did creating the journey repo clarify the shape of the whole arc?

> Scaffold the debrief: "Append a new debrief entry for Session 0.1-lite to docs/debriefs.md using the syllabus §4 template. Fill in date and session number; leave my reflection answers blank."

### Session 0.0 — Machine Setup (4-5 hrs)

**Objective:** By end of session, my Mac is fully configured for the next 10 weeks of work: tools installed, accounts created, Claude.ai workspace organized, folder structure in place, and a successful end-to-end smoke test (Claude Code creates a file in my repo, I see it in Cursor).

**Concept of the session:** Tool taxonomy. Before installing anything, I need to understand what each piece is for and how they fit together. The taxonomy: my machine (Terminal, Homebrew, Node, Git, Cursor) → my agent (Claude Code, authenticated to Anthropic) → my cloud accounts (GitHub for code hosting, Supabase for DB+auth, Vercel for hosting, Anthropic API for LLM calls) → my think tank (Claude.ai with a Project containing the syllabus and session-planning prompt).

**Pre-work (Claude.ai):** None. This session IS the pre-work for everything else.

**Build (interactive, with Claude.ai as your setup partner):**

Use the dedicated Session 0.0 prompt (separate document) to drive an interactive, step-by-step setup conversation. The prompt enforces:
- Tool taxonomy explanation BEFORE installation
- One command at a time, with plain-English explanation
- Wait-for-confirmation between steps
- Proactive warnings about common Mac gotchas
- A final end-to-end smoke test

**Verification (the smoke test, 20-30 min):**
- [ ] Terminal opens; you know what `pwd`, `cd`, `ls` do
- [ ] `brew --version` returns a version number
- [ ] `node --version` returns a version number
- [ ] `git --version` returns a version number
- [ ] `git config --global user.name` and `user.email` return your details
- [ ] Cursor (or VS Code) opens; you can open a folder in it
- [ ] `claude` launches Claude Code from Terminal
- [ ] Claude Code can create a file in `~/code/ai-engineering/playground/`
- [ ] You can see that file in Cursor
- [ ] GitHub account is live, SSH key registered, you can `ssh -T git@github.com` and get a success message
- [ ] Anthropic API key is saved somewhere secure (password manager recommended) — NOT in any code file
- [ ] Supabase and Vercel accounts exist and are linked to your GitHub
- [ ] Claude.ai Project "Native AI Engineering Syllabus" exists with syllabus_v2.md and session_planning_prompt.md attached, plus written project instructions

**Review pass:** Re-read the list above one day later. If any item is fuzzy ("did I actually do that?"), open Terminal and verify with the command. Don't move to Session 0.1 until every box is checked.

**Debrief prompts:**
- Which step took longer than expected? Why?
- Which step was scarier than it needed to be? Less scary?
- Which tool's purpose is still hand-wavy to me? (Note it — we'll demystify it when you actually use it.)
- Did I copy-paste anything I didn't understand? (Be honest. Note what.)
- Am I ready for Session 0.1, or do I want to spend 30 min tomorrow re-running smoke tests to feel solid?

### Session 0.1 — Workspace & Tool Taxonomy (4-5 hrs)

**Objective:** Working Claude Code on your Mac, GitHub account ready, editor configured, a "hello world" repo committed.

**Concept of the session:** What each tool is for, and the difference between "Claude as chat partner" (Claude.ai) and "Claude as engineer at your keyboard" (Claude Code). Why both exist.

**Pre-work (Claude.ai):**
- Ask Claude.ai for a current overview of: terminal basics on macOS, what git is at a conceptual level, what a code editor does, what Claude Code is. Get a one-page primer in plain English.
- Read [Appendix B](#appendix-b-tool-stack--setup) below.

**Build (Claude Code itself, once installed, will guide you):**
1. Install Claude Code (follow current official docs)
2. Install Cursor or VS Code (Cursor recommended — better AI integration)
3. Create a GitHub account if you don't have one. Generate an SSH key (Claude.ai or Claude Code will walk you through).
4. Create a new empty GitHub repo called `playground`. Clone it locally.
5. Open it in Cursor. Open the integrated terminal.
6. Launch Claude Code in that terminal. Ask it: "Create a single-page hello-world Next.js app in this folder, then commit and push."
7. Watch what it does. Read every command it runs. If you don't understand a command, ask Claude.ai before approving.

**Verification:**
- [ ] You can run `claude` in a terminal and it starts
- [ ] You can run `git status` and see the state of your repo
- [ ] Your hello-world is on GitHub and runs locally (`npm run dev`)
- [ ] You can articulate what each tool is for in one sentence

**Review pass:** Look at the files Claude Code created. Open `package.json`. Open `app/page.tsx`. For each file, ask Claude.ai: "explain this file to me line by line, assume I'm new." Don't move on until you can describe each file's purpose.

**Debrief prompts:**
- What commands did Claude Code run that surprised you?
- What was scarier than expected? Less scary than expected?
- One question you still have.

---

### Session 0.2 — Spec Craft (4-5 hrs)

**Objective:** A working understanding of PRDs, ADRs, and tech design docs, with templates you'll reuse for every project. By end of session: you've drafted a practice PRD for a fake feature ("add comments to my hello-world page") just to feel the muscle.

**Concept of the session:** The hierarchy of design artifacts and what each one is *for*.
- **PRD (Product Requirements Doc):** what we're building and why. Audience: anyone who needs to know what this product does. No code. Goals, non-goals, users, flows, success criteria.
- **Tech Design Doc:** how we're going to build it. Audience: engineers (you + agents). Stack, file structure, data model, API shape, key decisions.
- **ADR (Architecture Decision Record):** one decision, captured. Audience: future you and future agents. Context, decision, alternatives considered, consequences. Short. Numbered (ADR-001, 002...).
- **Spec / feature spec:** smaller than PRD, narrower than tech design. For one feature.

You write the PRD. Claude.ai helps you draft the tech design. Both you and Claude.ai produce ADRs whenever a non-obvious decision is made.

**Pre-work (Claude.ai):**
- Read [Appendix A](#appendix-a-artifact-templates) below — the templates.
- Have Claude.ai explain when to use each artifact. Ask for an example from a real-world product you'd know (e.g., Stripe Connect, Notion AI). Get a feel.

**Build (you, with Claude.ai as the drafting partner):**
1. Open Claude.ai. Paste the PRD template from Appendix A.
2. Imagine you're adding a "comments" feature to your playground app. Write a one-paragraph brief.
3. Ask Claude.ai: "Turn this brief into a filled-out PRD using the template. Ask me clarifying questions before filling in any section you're unsure about."
4. Answer its questions. Iterate. End up with a real PRD-shaped document.
5. Repeat for a tech design doc.
6. Write one ADR by hand: "ADR-001: Comments are anonymous, identified by stable UUID, not by user-provided name."
7. Save all three artifacts to `playground/docs/`. Commit.

**Verification:**
- [ ] You have three artifacts in `playground/docs/` matching the template structure
- [ ] You can explain the difference between PRD and tech design in one breath
- [ ] You can explain why ADRs are short and numbered

**Review pass (Claude.ai):**
- Paste your PRD into Claude.ai. Ask: "What's missing? What's ambiguous? What would an engineer push back on?"
- Note the gaps. These are your tells — the things you'll forget on your real PRDs too.

**Debrief prompts:**
- Which artifact felt most natural? Which felt forced?
- What did Claude.ai catch in your draft that you missed?
- Your existing spec instincts — what carries over, what's rusty?

---

### Session 0.3 — The Loop, Dry Run (4-5 hrs)

**Objective:** Run the full dual-loop on a throwaway feature in `playground`. By end of session you've felt every step: brief → PRD → tech design → CLAUDE.md task → Claude Code build → review → debrief.

**Concept of the session:** The full loop in one session, on something small enough not to matter. The mistakes you make here are free.

**Pre-work (Claude.ai):**
- Use the PRD from 0.2 as your starting point. Refine it slightly.
- Draft a CLAUDE.md for your `playground` repo using the template in Appendix B.

**Build (the full loop, end-to-end):**
1. Save the PRD and tech design to `playground/docs/`.
2. Save CLAUDE.md to `playground/` root.
3. Launch Claude Code in `playground/`.
4. Prompt it: "Read CLAUDE.md and docs/. Then propose a task breakdown for implementing the Comments feature. Don't write code yet."
5. Review its task breakdown. Push back on anything that smells off (premature abstraction, missing tests, unclear primary keys).
6. Approve and have it implement task 1.
7. Review the diff in your editor. Use the Oversight Checklist — at minimum the Design Sanity and Code Hygiene rows.
8. Paste the diff into Claude.ai for a review pass.
9. Apply any fixes in Claude Code.
10. Commit. Repeat for task 2 if time allows.

**Verification:**
- [ ] You felt every step of the loop
- [ ] You used Claude.ai and Claude Code for the right things at the right times
- [ ] You ran at least one Oversight Checklist pass on a real diff
- [ ] You caught at least one issue Claude Code missed (or note: it didn't miss anything this round)

**Review pass:** Re-read the diff one day later (if possible). Does it still make sense? Is there anything you'd rewrite?

**Debrief prompts:**
- Where did the loop feel natural? Where did it feel forced?
- Did you ever skip a step (e.g., jump from brief to Claude Code without the PRD)? What happened?
- What's the smallest change you'd make to your CLAUDE.md based on this session?
- Are you ready for Project 1, or do you want a Session 0.4 to drill another loop?

---

## 7. Project 1: Thesis Tracker

**~8 sessions across ~3 weeks**

**The product:** A web app where you track active investment theses. For each thesis: title, hypothesis, evidence for, evidence against, related companies/links, current confidence level, last updated. List view, detail view, edit, archive. Authenticated, single-user-per-account, multi-tenant isolation.

**Why this project:** Pure CRUD + auth + tenancy. No external integrations, no LLM pipeline. You learn the loop end-to-end on something small enough to ship in three weeks of sessions.

**Carries forward:** auth scaffolding, multi-tenancy patterns, deploy pattern, UI component library, CLAUDE.md template, test patterns, debrief habit.

**No evals in this project.** Project 1 is pure CRUD. There is no LLM output to score. Evals are a Project 3 deliverable. Session 1.8 also introduces Skills (Claude Code) and Codex Entry 1 (comparative re-implementation) — see §5b.

### Sessions

| # | Name | Ships | Foundation concept introduced |
|---|---|---|---|
| 1.1 | Spec & repo init | PRD, tech design, ADR-001 (UUIDs), repo + CLAUDE.md committed | What "good spec" looks like for your projects |
| 1.2 | Scaffold + auth | Next.js app with Google auth, deployed to Vercel | OAuth flow, env vars, deploy pipeline |
| 1.3 | Data model + tenancy | Postgres schema (theses, users), row-level security, tests | What RLS is and why "DB-level + app-level" is the right answer |
| 1.4 | CRUD UI (list + detail) | List page, detail page, both wired to DB | Server components vs client components |
| 1.5 | CRUD UI (create + edit) | New thesis form, edit form, validation | Form validation, optimistic UI |
| 1.6 | Polish + tests | Loading states, error states, full auth + tenancy test suite | Testing patterns; "what would I be embarrassed to ship" |
| 1.7 | Deploy + light red-team | Live URL, run 3 attacks against it yourself | Threat modeling, basic adversarial thinking |
| 1.8 | Debrief + retrospective | `docs/01-retro.md`, updated CLAUDE.md template, lessons file | Pattern extraction; what carries to Project 2 |

When you reach this project, ask me to flesh out any of these into full session plans (using the Session Template).

---

## 8. Project 2: Deal-Flow CRM

**~10 sessions across ~3 weeks**

> ⚠️ **SCOPE CREEP WARNING — READ BEFORE SESSION 2.1.**
> You've explicitly identified scope creep as your tendency on personal builds. Project 2 is the most likely point of failure for this entire syllabus because integrations are time sinks. Your **only** job at Session 2.1 is to narrow before you build.
>
> **Default narrowed scope (start here):** Companies + Contacts + Notes only. Gmail read-only, one-way sync, manual trigger. No Calendar. No scoring rubric in v1. Ship that, *then* decide if you have time for more.
>
> **Anti-creep checkpoint at Session 2.1:** before writing a single PRD line, list the 5 features you most want. Cross out the bottom 3. The remaining 2 are v1. The other 3 are v2 (which you will not build during this syllabus). If you cannot cross out 3, you are about to scope-creep — stop and call me. Ship-on-time matters more than feature-completeness here because Project 3 needs your full energy for the red-team.

**The product (narrowed v1):** Track companies and contacts with notes. Gmail read-only integration: user triggers a sync, app pulls threads associated with tagged contacts. That's it.

**The product (full vision, for later):** Add Calendar sync, automatic background sync, thesis-fit scoring rubric, ranked dashboard. None of these in v1.

**Why this project:** Adds external integrations (OAuth to Google APIs, sync), more complex data model (many-to-many relationships), and a richer trust boundary surface. Still no LLM pipeline.

**No evals here either.** Project 2 has integrations, not LLM output. Evals wait for Project 3. **Codex Entry 2 begins:** from Session 2.1, Codex `/review` joins your review pass as a second-model reviewer (see §5b). This project runs 8 sessions on the narrowed v1 scope (2.1–2.8), down from v2's 10 — Calendar and scoring are cut.

**Carries forward from P1:** auth, RLS, deploy, UI components, CLAUDE.md, test patterns.

**New layers:**
- Third-party OAuth (Gmail/Calendar API scopes)
- Webhooks and event-driven sync
- Polling vs push, sync vs async
- Tokens, refresh tokens, secret rotation
- Integration security (the attack surface expands considerably)

### Sessions

| # | Name | Ships | Foundation concept |
|---|---|---|---|
| 2.1 | Spec & decisions | PRD, tech design, ADR-002 (sync strategy), ADR-003 (scope of Gmail access) | Scoping integrations narrowly = security |
| 2.2 | Repo + auth + scaffold | New repo, reuse auth from P1, deployed | Carrying scaffolding between projects |
| 2.3 | Core data model | Companies, contacts, stages, notes, relationships | Many-to-many; junction tables |
| 2.4 | CRUD core | List + detail + edit for companies and contacts | Velocity gains from P1 muscle memory |
| 2.5 | Gmail OAuth + read scope | User can connect Gmail; we can read their threads | OAuth scopes; "least privilege" |
| 2.6 | Gmail sync | Background job pulls threads tagged with contacts | Async jobs, idempotency, dedup |
| 2.7 | Polish + tests + integration threat model | Loading/error states, full test suite, threat doc | Integration-specific threats: token theft, scope creep, replay |
| 2.8 | Deploy + medium red-team | Live URL, run 5 attacks (incl. token-related) | Stepping up adversarial sophistication |

**Cut from v2:** Calendar OAuth/sync and thesis-fit scoring. Both are v2-vision, not narrowed-v1. They go to projects-backlog.md. If you feel the pull to add them back, that is the scope-creep failure mode firing — the anti-creep checkpoint at 2.1 exists to catch exactly this.

Same pattern: ask me to flesh sessions as you get there.

---

## 9. Project 3: Research Triage + Red Team

**~12 sessions across ~3-4 weeks**

**The product:** Upload PDFs / paste URLs / drop transcripts. App runs your systems-chain prompt against each, stores the analysis, tags by thesis, makes everything searchable. Multi-tenant, deployed, hardened.

**Why this project:** Adds the LLM pipeline (cost, latency, prompt injection), file uploads (large attack surface), async job processing, and is the target for the full Karpathy red-team campaign.

**Carries forward from P2:** auth, RLS, deploy, UI components, integration patterns, async job patterns, test patterns, CLAUDE.md.

**New layers:**
- LLM API integration with cost tracking and per-user rate limiting
- File upload (size limits, type validation, sandboxed storage)
- Prompt injection defense (delimiters, instruction hierarchy, output validation)
- The full adversarial red-team campaign

### Sessions

| # | Name | Ships | Foundation concept |
|---|---|---|---|
| 3.1 | Spec & threat model | PRD, tech design, ADR-004 (prompt injection defense), threat model doc | LLM-app-specific risks |
| 3.2 | Repo + auth + scaffold | New repo, P2's auth+UI carried over, deployed | Patterns at full velocity now |
| 3.3 | Data model + tenancy | Documents, analyses, tags, full RLS | RLS in an LLM-heavy app |
| 3.4 | File upload + storage | Secure upload, size/type validation, signed URLs | File-upload threat surface |
| 3.5 | LLM call wrapper + first eval | Single LLM call works, cost logged, one eval passes | Evals vs unit tests (the core distinction) |
| 3.5b | Golden dataset + grader | 10 real inputs with known-good outputs, grader scores all 10 | Golden dataset as ground truth; your taste as signal |
| 3.6 | Async job pipeline | Background job: upload → extract text → LLM call → store result | Job queue, retries, dead letters |
| 3.7 | Prompt injection defense | User content quoted/delimited; output validated; system instructions hardened | The injection surface; why this matters |
| 3.8 | UI: analysis viewer + tagging | Document detail page, tag UI, search | Information density, taste pass |
| 3.9 | Eval suite + polish | Drift detection, logged failure modes, regression tests; then loading/error/retry UX | Drift detection; evals first, polish second |
| 3.10 | Full threat model + hardening | All inputs mapped, all trust boundaries, all privileged actions | OWASP-style at production grade |
| 3.11 | **Red-team campaign (the Karpathy moment)** | Mixed panel: 5–10 parallel Claude Code AND Codex subagents attack; log every attempt with which vendor found it, fix every break, regression-test every fix | Adversarial mindset; how model families differ as attackers |
| 3.12 | Final hardening + ship | Last-pass fixes, public README, retro doc | Closing the loop on the whole arc |

**Project 3 has no evals-free excuse and no evals-early temptation.** The three eval sessions (3.5, 3.5b, 3.9) are written out in full below. Each opens with a scope gate. Add "Eval note: what did the suite catch that human review missed, or vice versa?" to every Project 3 debrief.

### Eval sessions, written out

**Session 3.5 — LLM call wrapper + first eval.** Scope gate: if 3.1–3.4 aren't shipped, stop; there's nothing to evaluate yet. Concept: an eval asserts non-deterministic LLM output meets a standard via a tolerant grader; a unit test asserts deterministic code returns a fixed value. Build: wrap one Anthropic call with cost logging; store input/output/cost; write one eval checking the output contains the fields your systems-chain prompt should produce. ADR: how we grade (exact match too brittle; choose rubric or field-presence). **Failure mode:** Claude Code will propose exact-string-equality grading. It passes once, fails forever. Insist on rubric or field-presence. This is the MenuGen lesson in eval form.

**Session 3.5b — Golden dataset + grader.** Scope gate: 10 inputs is the target, not 50. Past 15, you're polishing the dataset instead of shipping the harness; cross out the extras. Concept: a golden dataset is human-curated ground truth, and the human is you — your capital-allocator judgment becomes the training signal. Build: store 10 real inputs (not synthetic) with expected outputs; extend the grader to score all 10; print pass/fail summary. At least one should fail on day one — a suite where everything passes is too easy to be useful. **Failure mode:** Claude Code will offer to generate synthetic examples. Refuse. Synthetic inputs test the model against its own assumptions, not reality.

**Session 3.9 — Eval suite + polish.** Scope gate: the eval suite is the priority; polish is secondary. Out of time means ship the suite, defer the spinner. Concept: drift detection runs the golden dataset before and after a change and compares scores, catching a change that quietly made things worse. Build: before/after scoring with a shown delta; log each failure mode as a named entry; add a regression case for every fix; then the polish. **Failure mode:** Claude Code treats a passing run as done. Make it fail on a deliberately broken prompt before you trust it. An eval suite that never fails isn't protecting you. This is the portfolio artifact — the eval line on your resume, the thing you whiteboard in an FDE round.

---

## 9b. Project 4: Travel Influencer Agent (the transfer proof)

Not a syllabus project. The proof the playbook transfers. **Spec'd in consolidation week (Week 7) using the finished playbook; built in September, MVP target end of September.**

**The product:** An AI-native agent that ingests saved content (YouTube transcripts via OAuth, podcasts, articles) into your Open Brain, then generates cited thought-leadership on your behalf. This is the travel-niche version; the general version (growth agent, thought-capture) is backlog as capstone v2.

**Why it's Project 4, not a syllabus swap:** you froze the roster for repeatability. Project 4 inherits everything — eval suite pattern, injection defense, tenancy, OAuth ingestion, async jobs — and the portfolio story is the delta: fourth project, fraction of the time, spec-first from day one. That "built in a third of the time using my own playbook" is a stronger claim than a fourth in-syllabus build.

**Two carried ADRs to write at spec time:**
- Ingestion via OAuth (YouTube read-only scope), not scraping. Restores the full consent/refresh-token/least-privilege surface that the narrowed Project 2 taught.
- Sync strategy (poll vs push), with idempotency and dedup so re-runs don't double-ingest.

**Build it multi-tenant even as a single user**, because "AI-native services infrastructure" means this becomes a product, and tenancy retrofits are miserable.

**Anti-creep at spec time:** list the five features you most want, cross out three before writing a PRD line. Emotional investment in this project makes your scope-creep failure mode strongest here.

---

## 10. Final Consolidation

**~2-3 sessions across a final week**

**Ship:**
- A consolidated personal playbook: refined CLAUDE.md template, refined Oversight Checklist (with your additions from 30+ sessions of debriefs), refined session template
- A `lessons.md` that distills your strongest insights — agent strengths, agent failure modes, your own taste signature, the patterns you keep hitting
- A blog-post-length write-up of the journey (publish or don't — the writing IS the consolidation)
- Three live URLs, three READMEs, three retros

This is where you can pause, look back, and confirm what you've actually built. The deliverable is not just three apps — it's a transferable skill set you'll apply to Project 4, 5, 6.

---

## 11. The RL Investment Map

Karpathy's hint: *"There is one domain that is very valuable, and it's not part of the mix yet."*

### What labs ARE heavily RL'ing
| Capability | Why RL-friendly | Your stance |
|---|---|---|
| Code that compiles + tests pass | Verifiable (binary) | Trust agents. Review for taste, not correctness. |
| Math & formal reasoning | Verifiable | Trust agents. |
| Tool use / agentic loops | Verifiable (did task complete) | Improving fast — Claude Code itself is proof. |
| Computer use (browser, OS) | Verifiable end-states | Trust for well-scoped tasks; verify side effects. |
| Long-context retrieval | Eval-able | Trust with caveat: confirm grounding. |

### What labs are NOT heavily RL'ing — your moat
| Capability | Why hard to RL | Your stance |
|---|---|---|
| Code aesthetics / refactor judgment | No clean reward signal | **Your taste is the moat.** |
| Architectural design / identity modeling | Verifiable too late (after the bug ships) | **Your spec discipline catches MenuGen-class bugs.** |
| Product / UX taste | Subjective | Yours. |
| Long-horizon multi-week project execution | Hard to construct realistic environments | Emerging. |
| Adversarial robustness on real codebases | No standardized environment | **My best read on Karpathy's hidden gem.** |

### The hidden gem (my read, with humility)

Karpathy didn't name it. The strongest signal is "verifiable + valuable + not in the mix yet." My best read: **adversarial software engineering at the project level** — exactly the test he describes. An RL environment where:
- Model builds a project against a spec
- Panel of adversarial agents tries to break it
- Reward: spec compliance + survives N adversaries + human-rated quality

That environment doesn't exist as a benchmark. Building it would unlock a class of training labs aren't currently running.

Closely related candidates:
- **Prompt injection defense as RL domain** (hostile docs → model has to stay aligned with user instructions; verifiable, valuable, thinly trained publicly)
- **Realistic long-horizon project completion** (multi-day, multi-feature, with revisions; SWE-bench is a starting point, not the end)

### Why this matters for your syllabus

Your debrief notes will track which agent capabilities you can trust and which you can't. Over 30+ sessions you'll build your own RL investment map — empirically grounded in *your* projects. That map is itself an artifact worth keeping; it's how you'll know which Parchmount workflows to automate aggressively vs. keep human-in-the-loop indefinitely.

---

## 12. The Roadmap

July 13 to August 30, 2026. Seven weeks, 34 sessions, five per week. Dates are latest-start; starting early banks slack. If you begin 0.1-lite before July 13, completion pulls forward and buffer accumulates at the back where the red-team lives.

**Assumptions:** 30-40 total work hours/week covering investment work and syllabus; syllabus gets ~22-27 (five sessions), investment work flexes around fixed session blocks. Session 0.0 done. All repos public from first commit.

### Week 1 (Jul 13-19): Foundation closes, Project 1 opens
0.1-lite (loop, Plan Mode, journey repo), 0.2 (spec craft), 0.3 (loop dry run, first CLAUDE.md), 1.1 (Thesis Tracker spec + repo, CLAUDE.md committed), 1.2 (scaffold + auth, deployed).
Ships: journey repo live; thesis-tracker public with deployed auth skeleton.
Watch: Supabase publishable-key naming, Next.js 16 defaults, Vercel Hobby ADR.

### Week 2 (Jul 20-26): Project 1 core
1.3 (data model + RLS), 1.4 (list/detail UI), 1.5 (create/edit), 1.6 (polish + tests), 1.7 (deploy + 3 attacks).
Ships: working Thesis Tracker live, RLS enforced, tests green, red-team log.
Watch: 1.3 is the MenuGen checkpoint — UUID keys, never email. Pre-work: install + auth Codex before 1.8.

### Week 3 (Jul 27-Aug 2): Project 1 closes, Project 2 opens
1.8 (retro, HOT 8hr: Skills + Codex Entry 1), 2.1 (spec + anti-creep checkpoint; Codex /review joins), 2.2 (repo + carried auth), 2.3 (data model, many-to-many), 2.4 (CRUD core).
Ships: Project 1 retro + comparative debrief; deal-flow-crm live skeleton.
Watch: heaviest context week. If hot, Codex Entry 1 slips to a standalone half-session in Week 4. Comparison branch is disposable.

### Week 4 (Aug 3-9): Project 2 closes, Project 3 opens
2.5 (Gmail OAuth), 2.6 (Gmail sync), 2.7 (polish + threat model), 2.8 (deploy + 5 attacks), 3.1 (Research Triage spec + threat model, injection ADR).
Ships: working CRM live with Gmail sync; research-triage repo initialized.
Watch: Gmail OAuth is the timeline sink. Past one session, invoke fallback: ship what works, log the ADR, move. Log reviewer disagreements, act only on checklist-flagged ones.

### Week 5 (Aug 10-16): Project 3 pipeline
3.2 (scaffold), 3.3 (data model + RLS), 3.4 (file upload, sandboxed), 3.5 (LLM wrapper + first eval), 3.5b (golden dataset + grader).
Ships: secure upload, one LLM call with cost logging, 10 golden examples + grader.
Watch: refuse exact-match grading (3.5) and synthetic examples (3.5b). Curate 10 real docs from your investment flow — doubles as investment work.

### Week 6 (Aug 17-23): Project 3 hardening
3.6 (async jobs), 3.7 (injection defense), 3.8 (viewer + tagging + search), 3.9 (eval suite + polish), 3.10 (full threat model).
Ships: complete pipeline, injection defenses, drift-detecting eval suite, threat model.
Watch: 3.9 is evals-first. Make the suite fail on a broken prompt before trusting it. Debrief quality check this week.

### Week 7 (Aug 24-30): The Karpathy moment, then consolidation
3.11 (mixed-vendor red-team), 3.12 (final hardening, README, retro), consolidation A (playbook + lessons.md with vendor-comparative RL map), consolidation B (Project 4 spec + journey write-up).
Ships by Aug 30: three live URLs, three retros, mixed-vendor red-team log, lessons.md, playbook, Project 4 spec in a fresh public repo with zero code.
Watch: 3.11 is the payoff. It absorbs zero slip from earlier weeks.

### Slip rules
1. One behind: extend one session toward 8 hrs.
2. Two+ behind: cut from the back of Project 2, never Project 1 or the red-team.
3. Codex Entry 1 slips to Week 4 if Week 3 overloads; Entries 2 and 3 never slip.
4. Debrief quality floor: end of Week 6, reread Week 5 debriefs. If they read like checkboxes, drop to four sessions in Week 7 and extend three days into September. Capturing the repeatability is the product.

### Acceleration trigger
If hours run syllabus-heavy or investment work is light, do not exceed six sessions/week. Pull the calendar forward a week and start Project 4's build in the recovered time. The ceiling protects debrief-and-return retention.

### Standing weekly rhythm
Session blocks on the calendar first, investment work around them. Sunday, 15 min: reread the week's debriefs, confirm next week's five sessions, verify everything shipped is visible in the public repos including the journey repo.

---

## Appendix A: Artifact Templates

### PRD Template

```markdown
# PRD: [Feature/Product Name]
Author: ___ | Date: ___ | Status: [draft / in-review / approved]

## 1. Summary (3 sentences)
What we're building. For whom. The single most important thing it does.

## 2. Goals
- Bullet list. 3-5 max. Each goal is testable.

## 3. Non-Goals
- Bullet list. Explicit "we are NOT building X" prevents scope creep.

## 4. Users & Use Cases
Who uses this and in what situations. One paragraph each for 2-3 primary use cases.

## 5. User Flows
For each primary flow, a numbered list of steps from user perspective.
Flow A: Authenticated user creates a new thesis
  1. User clicks "New Thesis"
  2. Form appears with title, hypothesis, evidence fields
  3. User fills and clicks "Save"
  4. Thesis is created, user is redirected to detail page

## 6. Data Model (high-level)
What entities exist. What relationships. NOT schema — that's the tech design.
Example: "Users have many Theses. Each Thesis has many Evidence items. Evidence items belong to a Thesis."

## 7. Edge Cases & Open Questions
- What if X?
- Unclear: how do we handle Y?

## 8. Success Criteria
How will we know this is done and good?
- [ ] Can a user create, edit, archive a thesis?
- [ ] Two users cannot see each other's theses?
- [ ] App is deployed and reachable at custom domain?

## 9. Out of Scope (this version)
What we're explicitly punting to v2.
```

### Tech Design Template

```markdown
# Tech Design: [Feature/Product Name]
Author: ___ | Date: ___ | Reviewed by: ___ (you, with Claude.ai)

## 1. Context
One paragraph: what problem we're solving, link to PRD.

## 2. Stack
- Framework: ___
- DB: ___
- Auth: ___
- Hosting: ___
- Etc.

## 3. Data Model (schema-level)
For each table: columns, types, primary key, foreign keys, indexes.
Include the explicit "primary key is UUID, never derived from user input" rule.

## 4. API / Route Shape
List of routes/endpoints. Method, path, what it does, auth requirements.
E.g.:
- GET /api/theses — list current user's theses (auth required)
- POST /api/theses — create thesis (auth required)
- GET /api/theses/:id — get one thesis (auth + RLS)

## 5. Identity & Tenancy
Explicit:
- User identity is `users.id` (UUID), populated at signup, never changed
- Every user-owned row has `user_id` FK
- Tenancy enforced at: (a) database (RLS policies) AND (b) application middleware
- Auth provider mapping: Google OAuth → user lookup by `provider_subject`, NEVER by email

## 6. File / Folder Structure
Tree view of how the codebase is laid out.

## 7. Key Decisions (linked to ADRs)
- ADR-001: UUID PKs
- ADR-002: RLS strategy
- (link or summarize each)

## 8. Risks & Open Questions
What might go wrong. What's still TBD.
```

### ADR Template

```markdown
# ADR-NNN: [Title — the decision in 1 line]
Status: [proposed / accepted / superseded by ADR-XXX]
Date: ___

## Context
What's the situation that requires a decision? 2-3 sentences.

## Decision
What we decided. 1-2 sentences. Direct, declarative.

## Alternatives Considered
- Option A: ___ (rejected because ___)
- Option B: ___ (rejected because ___)

## Consequences
What this decision enables. What it costs. What future work it commits us to.
```

ADRs are short on purpose. One decision per ADR. Numbered sequentially. Live in `docs/adr/`.

---

## Appendix B: Tool Stack & Setup

### Your machine
- **Editor:** Cursor (recommended) or VS Code
- **Terminal:** Built-in macOS Terminal or iTerm2
- **Claude Code:** install via official Anthropic docs
- **Node.js:** latest LTS (Claude Code will tell you if missing)
- **Git:** preinstalled on macOS; Claude Code runs git commands for you

### Stack (all three projects)
- **Framework:** Next.js (App Router) + TypeScript
- **Styling:** Tailwind CSS + shadcn/ui
- **DB:** Supabase (Postgres + auth + row-level security)
- **LLM:** Anthropic API (Sonnet for most calls, Opus for high-stakes)
- **Hosting:** Vercel
- **Background jobs (Project 2+):** Inngest or Trigger.dev
- **File storage (Project 3):** Supabase Storage or Vercel Blob

### CLAUDE.md template (drop into every repo)

```markdown
# Project: [Name]

## Purpose
[One paragraph]

## Core Constraints (Claude Code must respect these — NEVER violate)
1. All entities use UUID primary keys. NEVER use email or any user-provided field as a primary key.
2. All user data scoped by user_id. Tenancy enforced at both DB (RLS) AND application middleware. NEVER trust client-supplied user_id.
3. All LLM calls rate-limited per user. NEVER allow unbounded LLM loops.
4. User-supplied content is untrusted in prompts. Use clear delimiters / instruction hierarchy.
5. Tests required for every auth path and every cross-tenant boundary.
6. Files >300 lines or functions >50 lines require justification in code review.
7. Secrets in env vars only, never in code or in test fixtures.
8. Default-deny auth: routes are auth-required unless explicitly marked public.

## Decisions Log
- ADR-001: [link]
- (add as project evolves)

## Current Phase
Session X.Y — [current focus]

## Conventions
- TypeScript strict mode on
- No `any` types without justification comment
- All async functions have explicit error handling
- All env vars documented in .env.example
```

### Suggested chunking
- Each chunk = one Session Template, 4-5 hrs minimum, 8 hrs extended
- 3 chunks per week target
- If you have a hot week, run 4 chunks; if spotty, run 1-2 — adjust calendar, not pace
- 30-36 sessions total, ~10 weeks calendar

---

## Closing

You already own the harder half: spec, taste, judgment. The next 10 weeks are about adding execution velocity through directed agentic work, with a quality bar you set and verify. By Project 3's red-team session, you'll either pass Karpathy's test or know exactly which sessions to revisit.

The transferable artifact isn't the three apps. It's the playbook you'll have built in your `docs/debriefs.md` across 30+ sessions — your personal map of what to trust agents with, what to own yourself, and how to compose them.

That map, once you have it, is what makes you a native AI engineer in Karpathy's sense.
