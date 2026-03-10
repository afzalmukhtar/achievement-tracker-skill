---
name: achievement-tracker
description: >
  Track every work contribution (no matter how small) and personal project achievement.
  Generate manager reports (weekly/monthly/quarterly/half-yearly/yearly) and resume bullets.
  Use whenever the user completes ANY task at work, pushes code, opens/merges a PR, writes tests,
  fixes bugs, reviews code, writes docs, helps a teammate, improves performance, or asks for a
  report or resume. Also use for personal project updates and summaries.
  Logs every entry using the IMPACT formula (Verb + Achievement + Metric + Stakes + Evidence + How)
  so resume bullets and manager reports write themselves from structured data.
---

# Achievement Tracker Skill

The purpose of this skill is **career visibility and promotion readiness**. Every single work task — even a one-line bug fix — gets logged using the IMPACT formula so resume bullets and manager reports write themselves. Personal projects get a rich profile that doubles as a resume entry.

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

**Question 1 — Storage location:**
> "Where would you like to store your achievements? This will be a private git repo you own.
> Default: `~/Documents/personal-projects/achievements` — press Enter to accept."

**Question 2 — Git setup:**
> "Do you already have a git repository for achievements, or would you like to set one up?
> [A] I have an existing repo — I'll provide the remote URL
> [B] Initialize a new git repo here — I'll provide the remote URL (or skip for now)
> [C] Keep local only — no git"

- If **A**: ask for remote URL → run `git init && git remote add origin <url>` in the folder
- If **B**: run `git init` → ask for remote URL → add remote if provided
- If **C**: skip git entirely

**Question 3 — Owner info:**
> "What's your full name?" and "What's your personal GitHub username?"

Then:
1. Create the folder at the path they chose (or default)
2. Create `work/` and `personal/` subfolders
3. Create `README.md` from the **Achievements README template** below
4. Write `config.json`
5. Confirm: *"✅ Achievement tracker initialized at `<path>`"*

**config.json schema:**
```json
{
  "achievements_root": "<absolute path the user chose>",
  "owner": "<full name>",
  "github_username": "<personal GitHub username>",
  "ssh_host": "github.com-personal",
  "git": {
    "mode": "existing | new | none",
    "remote": "<remote URL or empty>"
  },
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
| Any work completed: code pushed, PR opened/merged, bug fixed, test written, review done, doc written, improvement made, ticket closed | **Log Work Entry** |
| "log this" / "track this" / "note this" | **Log Work Entry** |
| Personal project updated or described | **Update Personal Profile** |
| "weekly report" / "what did I do this week" | **Report Workflow** — 7 days, work only |
| "monthly report" | **Report Workflow** — current month, work only |
| "quarterly report" / "Q1/Q2/Q3/Q4 report" | **Report Workflow** — quarter, work only |
| "half-yearly" / "H1/H2 report" | **Report Workflow** — 6 months, work only |
| "yearly report" | **Report Workflow** — full year, work only |
| "resume" / "resume bullets" / "update my resume" | **Resume Workflow** — all projects |
| "what did I do on <project>" | Summarize that project's LOG.md |
| "add project" / new project detected | Create folder + file + update README.md |
| "show my achievements" / "summary" | Summarize README.md + recent LOG entries |

---

## The IMPACT Formula

**Every LOG entry must capture these fields.** This is the source of truth for both resume bullets and manager reports — fill them in at the time of logging so nothing is lost.

The formula is derived from Google XYZ (recruiter-recommended), STAR, CAR, FAANG resume research, and analysis of high-performing engineering resume bullets. It goes beyond XYZ by adding **Stakes** and **Evidence** — the two elements that turn a good bullet into a great one.

```
[Power Verb] [Achievement] [Metric: from X→Y / Nx / %]
[Stakes: in/across/where context that elevates impact]
[Evidence: patent filing, production deployment, measurement]
by [How: technically specific tools, architecture, approach]
```

**The 5 elements:**

| Field | What it captures | Why it matters |
|-------|-----------------|----------------|
| **Verb** | Strong action word — Reduced, Scaled, Eliminated, Built, Shipped, Designed | Sets the tone; recruiters scan first words |
| **Achievement** | What changed in one phrase | The headline of the bullet |
| **Metric** | Before→after / multiplier / % / count | Quantification is what separates strong bullets from weak ones |
| **Stakes** | Scale, users affected, risk context, business criticality | Elevates impact — "in prod where outages affect millions" > "in prod" |
| **Evidence** | Patent filings, production deployments, measurements, tickets | Makes claims verifiable |
| **How** | Technically specific method: named tools, architecture, decision | Shows depth; distinguishes senior engineers |

---

## Logging Work Entries

**Log every single thing, no matter how small.** A one-line bug fix that prevents a production incident is worth more than it looks. Visibility is promotion.

### When to log (never miss these)
- Fixed any bug — even a config typo or one-liner
- Opened, reviewed, merged, or commented on a PR
- Wrote or updated any test
- Improved performance, latency, throughput, or memory in any way
- Wrote or updated documentation
- Helped or unblocked a teammate
- Closed a Jira ticket or sub-task (any size)
- Ran a benchmark, spike, or investigation with a conclusion
- Set up tooling, CI, or infrastructure
- Made an architectural decision or recommendation
- Refactored for quality, readability, or maintainability
- Anything that ships to production

### LOG entry format

```markdown
## YYYY-MM-DD — [Category] ≤6-word title

**Verb:** <Reduced / Scaled / Eliminated / Built / Shipped / Designed / Refactored / Closed / Improved>
**Achievement:** <what changed — one phrase, no verb>
**Metric:** <before→after / Nx multiplier / % / count / time saved — or "qualitative" if not measurable>
**Stakes:** <scale, users, products, risk context — skip if minor>
**Evidence:** <patent filing / production deployment / PR / measurement — skip if none>
**How:** <technically specific: named tools, frameworks, architecture, decision>
**Tags:** #perf #bugfix #pr #test #refactor #docs #review #infra #research #team #setup
**Jira:** KEY-XXXXX
**PR:** #NNN
**Missed/Blocked:** <what slipped, failed, or is at risk — skip if none>
**Doc:** <PR link, runbook, Confluence, design doc — skip if none>
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
- Determine from context: open files, Jira ticket, branch name, user description
- If folder doesn't exist → create `work/<project-slug>/LOG.md` and update `README.md`
- If ambiguous → ask: *"Which project should I log this under?"*

### Example entry (IMPACT format)

```markdown
## 2026-03-10 — [Perf] Metadata dedup O(n²) → O(n)

**Verb:** Refactored
**Achievement:** metadata merge deduplication from O(n²) to O(n)
**Metric:** 2.3s → 0.1s on 10k-doc corpus (23x improvement)
**Stakes:** across all post-processing pipelines in production docs chunker
**Evidence:** microbenchmark with realistic 10k-doc corpus
**How:** replaced list-scan membership checks with order-preserving set-backed merge
**Tags:** #perf #refactor
**Jira:** AIPDQ-13501
**PR:** #412
```

*Resume bullet generated from this entry:*
> "Refactored metadata deduplication from O(n²) to O(n), cutting processing time 23x on 10k-doc corpus across all production post-processing pipelines"

---

## Personal Project Profiles

Personal projects use **PROFILE.md** — not a per-task log, but a living resume entry updated as the project evolves.

### PROFILE.md template

```markdown
# <Project Name>

> **Status:** Active | Completed | On Hold
> **Repo:** https://github.com/<username>/<repo>
> **Started:** YYYY-MM

## What it does
One to two sentences — what the project does and the problem it solves.

## Why it matters
Who uses it, what impact it has, what real-world problem it addresses.

## Tech Stack
- **Language:** Python / TypeScript / etc.
- **Frameworks:** FastAPI, LangChain, React, etc.
- **AI/ML:** LLM APIs, embeddings, vector DBs, etc.
- **Infra:** Docker, GitHub Actions, etc.

## Key Achievements
- <Verb> <what was built/improved> — <result or metric>
- <Verb> <what was built/improved> — <result or metric>
- <N> users / <N> stars / <N> downloads (if applicable)

## Skills Demonstrated
`Python` `LLM Engineering` `RAG` `Prompt Engineering` `Vector Search` ...

## Resume Bullet
_One polished IMPACT-formula sentence: [Verb] [Achievement] [Metric/Stakes] by [How]_
```

---

## Report Workflow

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
- Parse entries by `## YYYY-MM-DD` date headers
- Keep entries in range; group by category tag

### Step 3 — Generate report

```markdown
# Work Report — [Period]
> Generated: YYYY-MM-DD | Period: [range] | Entries: N

## Highlights (top 3–5)
_Written as manager-ready IMPACT sentences. Lead with outcome + metric, add stakes where available._
- [Verb] [Achievement] [Metric] [Stakes] by [How] — (JIRA-KEY)

## Completed This Period
| Date | Category | Achievement | Metric | Evidence |
|------|----------|-------------|--------|----------|
| YYYY-MM-DD | [Perf] | Title | before→after | PR #NNN / JIRA-KEY |

## In Progress
| Task | Status | ETA |
|------|--------|-----|
| JIRA-XXX: Title | % complete / current state | Date |

## Missed / Delayed / Blocked
| Task | Reason | Mitigation | Impact to Team |
|------|--------|------------|----------------|
| JIRA-XXX: Title | Why it slipped | Plan to recover | Low / Med / High |

## Risks & Open Items
- [Risk description] → [Mitigation plan] → [Expected resolution date]

## Documentation Trail
- [What shipped] → [Where documented: PR / Confluence / Jira / runbook]

---
**Total:** N entries | N PRs merged | N bugs fixed | N tests written | N perf wins
```

**Report writing rules:**
- Active voice, outcome first
- ✅ "Reduced chunking latency from 2.3s to 0.1s by implementing asyncio parallelization"
- ❌ "Worked on chunker improvements"
- Use SPIKE framing for bad news: "Delayed [X] by [N days] due to [specific cause]. Mitigation: [Y]. Expected impact: [Z]. [Adjacent deliverable] unaffected."
- Omit empty sections
- Include Jira/PR links wherever available

---

## Resume Workflow

### Step 1 — Read all data
- Read all `work/<project>/LOG.md` files
- Read all `personal/<project>/PROFILE.md` files

### Step 2 — Apply IMPACT formula to generate bullets

**Formula:** `[Verb] [Achievement] [Metric: before→after or Nx or %] [Stakes] by [How]`

**Rules (non-negotiable):**
- ≤25 words per bullet, one line — if it needs two lines, split into two bullets
- Outcome-first: never start with what you *did*, start with what *changed*
- Always prefer before→after over percentage alone ("15→24,000/day" > "1600x increase")
- Add stakes clause when impact affects users, products, risk, or scale
- Method must name specific tools/frameworks/approaches — never vague ("improved the system")
- Use strong past-tense verbs: Reduced, Eliminated, Scaled, Shipped, Refactored, Automated, Designed, Validated, Generated, Implemented

**Good vs bad:**
```
✅ "Scaled synthetic-data throughput 1600x — from 15 to 24,000 samples/day — by building a
   self-orchestrating LangGraph + LangChain generation ecosystem"

✅ "Reduced LLM hallucinations by 85% in production environments serving millions of users
   by designing text provenance verification + confidence scoring guardrail systems"

✅ "Improved Recall@30 from 0.63→0.85 and NDCG from 0.68→0.95 by introducing semantic
   chunking addressing ranking drift, header dominance, and embedding truncation"

❌ "Worked on improving data generation throughput"
❌ "Fixed hallucination issues in production"
❌ "Improved retrieval metrics using chunking optimizations"
```

### Step 3 — Generate resume section

```markdown
# Resume Bullets — [Date generated]

## Work — <Role / Company>
- [IMPACT bullet — highest metric wins]
- [IMPACT bullet]
- ...
_Pull 5–8 highest-impact entries. Prioritize: [Perf] > [Feature] > [Research] > [Test] > others._
_Group by theme if many: Performance, Features Shipped, Quality & Testing, Team Impact._

## Personal Projects

### <Project Name>
> <One-line what it does from PROFILE.md>
> **Stack:** <from PROFILE.md Tech Stack>

- <Key Achievement 1 in IMPACT format>
- <Key Achievement 2 in IMPACT format>

**Resume bullet:** <polished IMPACT sentence from PROFILE.md>
```

---

## Achievements README Template

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

- **Never overwrite** an existing LOG.md — always append new entries at the bottom
- **Never fabricate** entries — only log real work from the current session or explicitly described
- **Update README.md** whenever a new project folder is created
- **config.json is never committed** — stays local, in `.gitignore`
- When a Jira ticket or PR appears in conversation, proactively offer to log it
- For reports: if no entries exist in the range, say so — never generate an empty or fabricated report
- When Metric is unknown: write `qualitative` — never omit the field entirely
