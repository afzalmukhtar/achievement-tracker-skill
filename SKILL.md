---
name: achievement-tracker
description: >
  Track every work contribution (no matter how small) and personal project achievement.
  Generate manager reports (weekly/monthly/quarterly/half-yearly/yearly) and resume bullets.
  Use whenever the user completes ANY task at work, pushes code, opens/merges a PR, writes tests,
  fixes bugs, reviews code, writes docs, helps a teammate, improves performance, or asks for a
  report or resume. Also use for personal project updates and summaries.
---

# Achievement Tracker Skill

The purpose of this skill is career visibility and promotion readiness. Every single work task — even a one-line bug fix — gets logged. Personal projects get a rich profile that doubles as a resume entry. Everything feeds into on-demand manager reports and resume bullets.

---

## STEP 0 — Config Check (ALWAYS RUN FIRST)

Before doing **anything**, check for the config file:

**Config path:** `~/.codeium/windsurf/skills/achievement-tracker/config.json`

```
IF config.json does not exist:
  → Run the ONBOARDING FLOW below
ELSE:
  → Read config.json and use `achievements_root` as the base path for all operations
```

### Onboarding Flow (first time only)

Ask the user:

> "Where would you like to store your achievements? This folder will be a private git repo you own.
> Press Enter to use the default: `~/Documents/personal-projects/achievements`"

Then:
1. Create the folder at the path they provide (or default)
2. Create the subfolder structure: `work/` and `personal/`
3. Create `README.md` from the **Achievements README template** below
4. Write `config.json` with their answers
5. Confirm: *"✅ Achievement tracker initialized at `<path>`"*

**config.json schema:**
```json
{
  "achievements_root": "<absolute path the user chose>",
  "owner": "<ask: what's your full name?>",
  "github_username": "<ask: personal GitHub username?>",
  "ssh_host": "github.com-personal",
  "initialized": true
}
```

---

## Folder Structure

```
<achievements_root>/
├── README.md              ← master index, auto-updated when projects are added
├── work/
│   └── <project-slug>/
│       └── LOG.md         ← append-only log, one entry per task/achievement
└── personal/
    └── <project-slug>/
        └── PROFILE.md     ← resume-style project summary
```

---

## Routing Table

| User says / situation | Action |
|----------------------|--------|
| Config missing | Onboarding flow |
| Any work is completed (code pushed, PR opened/merged, bug fixed, test written, review done, doc written, task finished, improvement made) | **Log Work Entry** |
| "log this" / "track this" / "note this" | **Log Work Entry** |
| Personal project updated or described | **Update Personal Profile** |
| "weekly report" / "what did I do this week" | **Report Workflow** — 7 days, work only |
| "monthly report" | **Report Workflow** — current month, work only |
| "quarterly report" / "Q1/Q2/Q3/Q4 report" | **Report Workflow** — quarter, work only |
| "half-yearly" / "H1/H2 report" | **Report Workflow** — 6 months, work only |
| "yearly report" | **Report Workflow** — full year, work only |
| "resume" / "resume bullets" / "update my resume" | **Resume Workflow** — all projects |
| "what did I do on <project>" | Read that project's LOG.md and summarize |
| "add project" / new project detected | Create folder + file + update README.md |
| "show my achievements" / "summary" | Summarize README.md + recent LOG entries |

---

## Logging Work Entries

**Log every single thing, no matter how small.** A one-line bug fix that prevents a production incident is worth more than it looks. Visibility is promotion.

### When to log (do not miss these)
- Fixed any bug — even a typo in a config
- Opened, reviewed, merged, or commented on a PR
- Wrote or updated any test
- Improved performance in any measurable or qualitative way
- Wrote or updated documentation
- Helped or unblocked a teammate
- Completed a Jira ticket or sub-task (any size)
- Ran a benchmark, spike, or investigation with a conclusion
- Set up tooling, CI, or infrastructure
- Participated in a meaningful code review (as author or reviewer)
- Made an architectural decision or recommendation
- Refactored code for quality, readability, or maintainability

### Entry format

```markdown
## YYYY-MM-DD — [Category] Short descriptive title

**Impact:** What improved, what was fixed, what was unblocked, what was prevented
**Tags:** #bugfix #pr #perf #test #refactor #docs #review #infra #research #team #setup
**Metrics:** before → after  ← skip if not measurable
**Jira:** [KEY-XXXXX](https://org.atlassian.net/browse/KEY-XXXXX)  ← skip if none
**PR:** [#NNN](https://github.com/org/repo/pull/NNN)  ← skip if none

One to three sentences describing what was done, why it mattered, and any notable technical detail.
```

### Categories
| Tag | Use when |
|-----|----------|
| `[Bugfix]` | Any bug fixed |
| `[Perf]` | Latency, throughput, memory, CPU improvement |
| `[Feature]` | New functionality added |
| `[Test]` | Tests written or coverage improved |
| `[Refactor]` | Code quality, structure, or readability improved |
| `[Docs]` | Documentation written or updated |
| `[PR]` | Pull request opened, reviewed, or merged |
| `[Review]` | Meaningful code review given |
| `[Infra]` | CI/CD, build, tooling, deployment |
| `[Research]` | Spike, benchmark, investigation with a result |
| `[Team]` | Helped teammate, unblocked someone, knowledge share |
| `[Setup]` | Project, environment, or tool setup |

### Which project to log under
- Determine project from context (open files, Jira ticket, branch name, user description)
- If the project folder doesn't exist yet → create `work/<project-slug>/LOG.md` and update `README.md`
- If ambiguous → ask: *"Which project should I log this under?"*

### Example entry
```markdown
## 2026-03-10 — [Perf] Reduced metadata deduplication from O(n²) to O(n)

**Impact:** Eliminated quadratic slowdown in metadata merge for large document sets
**Tags:** #perf #refactor
**Metrics:** 2.3s → 0.1s on 10k-doc corpus
**Jira:** [AIPDQ-13501](https://cisco-sbg.atlassian.net/browse/AIPDQ-13501)
**PR:** [#412](https://github.com/org/ai-common-py/pull/412)

Replaced list-scan deduplication in the metadata merge path with an order-preserving set-backed
implementation. Previously O(n²) due to repeated membership checks; now O(n). Validated with
microbenchmark showing 23x improvement on realistic corpus sizes.
```

---

## Personal Project Profiles

Personal projects use a **PROFILE.md** instead of LOG.md. This is not a per-task log — it's a living resume entry that you update as the project evolves.

### PROFILE.md template
```markdown
# <Project Name>

> **Status:** Active | Completed | On Hold
> **Repo:** https://github.com/afzalmukhtar/<repo>
> **Started:** YYYY-MM

## What it does
One to two sentence explanation of what this project does and the problem it solves.

## Why it matters
Who uses it, what impact it has, what problem in the world it addresses.

## Tech Stack
- **Language:** Python / TypeScript / etc.
- **Frameworks:** FastAPI, LangChain, React, etc.
- **AI/ML:** LLM APIs, embeddings, vector DBs, etc.
- **Infra:** Docker, GitHub Actions, etc.

## Key Achievements
- Built X which does Y — resulted in Z
- Reduced <metric> from A to B
- <N> users / <N> stars / <N> downloads (if applicable)

## Skills Demonstrated
`Python` `LLM Engineering` `API Design` `Prompt Engineering` `Vector Search` ...

## Resume Bullet
_One polished past-tense sentence suitable for a resume._
```

---

## Report Workflow

When the user asks for any time-based report:

### Step 1 — Determine date range
| Report | Range |
|--------|-------|
| Weekly | Last 7 calendar days (or Mon–Sun of specified week) |
| Monthly | Current or specified calendar month |
| Quarterly | Q1=Jan–Mar, Q2=Apr–Jun, Q3=Jul–Sep, Q4=Oct–Dec |
| Half-yearly | H1=Jan–Jun, H2=Jul–Dec |
| Yearly | Full calendar year |

### Step 2 — Read and filter
- Read every `work/<project>/LOG.md`
- Parse entries by date header `## YYYY-MM-DD`
- Keep only entries in the date range
- Group by category tag

### Step 3 — Generate report

```markdown
# Work Report — [Period]
> Generated: YYYY-MM-DD | Entries: N

## Highlights
_3–5 most impactful bullets, written as manager-ready sentences. Lead with the metric or outcome._
- Reduced Firewall_OnPrem doc processing latency by ~60% via asyncio chunk-level parallelization
- Closed 4 critical test coverage gaps in the docs chunker library

## Performance & Optimization
- YYYY-MM-DD · [Perf] Title — Impact (Metrics if available) (JIRA-KEY)

## Features Shipped
- ...

## Bug Fixes
- ...

## Tests Written
- ...

## Code Reviews & Team Contributions
- ...

## Docs, Infra & Setup
- ...

---
**Total:** N entries | N PRs | N bug fixes | N tests | N perf wins
```

**Writing rules for reports:**
- Active voice, outcome first
- ✅ "Reduced chunking latency by 60% by implementing asyncio parallelization"
- ❌ "Worked on the chunker code"
- Include Jira/PR links when available
- Omit empty sections

---

## Resume Workflow

When the user asks for resume bullets or a resume update:

### Step 1 — Read all data
- Read all `work/<project>/LOG.md` files
- Read all `personal/<project>/PROFILE.md` files

### Step 2 — Generate resume section

```markdown
# Resume — Achievements & Projects

## Work Experience

### <Most recent role / project context>
- <Past tense, impact-first, quantified where possible>
- <Grouped by theme: performance, features, quality, etc.>

_Tip: Pull 5–8 of the highest-impact bullets from the LOG files. Prioritize [Perf] and [Feature] entries with Metrics._

## Personal Projects

### <Project Name>
<One-line description from PROFILE.md>
**Stack:** ...
- <Key Achievement 1>
- <Key Achievement 2>
**Resume bullet:** <polished single sentence from PROFILE.md>
```

**Resume writing rules:**
- Past tense ("built", "reduced", "implemented", "shipped")
- Lead with the result, not the action
- Include tech stack for personal projects
- Include numbers wherever the LOG has `**Metrics:**`

---

## Achievements README Template

Use this when initializing the achievements repo:

```markdown
# Achievements & Contributions

> **Owner:** <name>
> **Last updated:** YYYY-MM-DD

A private log of every work contribution and personal project achievement.
Used to generate manager reports, track career growth, and build resume content.

---

## Work Projects

| Project | Log | Latest entry |
|---------|-----|--------------|
| *(none yet)* | — | — |

---

## Personal Projects

| Project | Profile | Status |
|---------|---------|--------|
| *(none yet)* | — | — |

---

## Quick Stats
> Ask the agent: "give me a summary of my achievements"

- **Total work entries:** —
- **PRs contributed:** —
- **Perf improvements:** —
- **Tests written:** —
```

---

## Maintenance Rules

- **Never overwrite** an existing LOG.md — always append
- **Never fabricate** entries — only log real work
- **Update README.md** whenever a new project folder is created
- **config.json is never committed** — it stays local (in `.gitignore`)
- When a Jira ticket or PR is mentioned in conversation, offer to log it
- For reports: if no entries exist in the range, say so rather than generating an empty report
