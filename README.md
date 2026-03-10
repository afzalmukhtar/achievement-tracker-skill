# achievement-tracker-skill

A Windsurf AI skill for tracking every work contribution and personal project achievement — no matter how small — and generating manager reports and resume bullets on demand.

## What it does

- **Logs every work task** (bug fixes, PRs, tests, reviews, docs, perf improvements, team contributions) into per-project `LOG.md` files
- **Profiles personal projects** with tech stack, achievements, and resume-ready summaries
- **Generates reports** filtered by time range (weekly / monthly / quarterly / half-yearly / yearly) — work only, manager-ready language
- **Generates resume bullets** from all logged work + personal project profiles
- **Self-initializes** on first use — asks where you want to store achievements, creates the folder structure, writes a local `config.json`

## Installation

1. Clone into your Windsurf skills directory:
   ```bash
   cd ~/.codeium/windsurf/skills
   git clone git@github.com-personal:afzalmukhtar/achievement-tracker-skill.git achievement-tracker
   ```
   
2. Windsurf will automatically pick up the skill from the `achievement-tracker/` folder.

3. On first use, the skill will ask where to store your achievements and create a local `config.json` (never committed).

## First use

Just say anything like:
- *"log this"* — after completing any work task
- *"weekly report"* — get a manager-ready summary of the week
- *"update my resume"* — generate resume bullets from all logged work

The skill will guide you through setup on the very first interaction.

## Config

A `config.json` is created locally (gitignored) after first-time setup:

```json
{
  "achievements_root": "/path/to/your/achievements",
  "owner": "Your Name",
  "github_username": "yourusername",
  "ssh_host": "github.com-personal",
  "initialized": true
}
```

## Achievements storage

A **separate private GitHub repository** called `achievements` holds all your accomplishments and achievements. You create this once during onboarding — it stays private and is yours to push to whenever you want.

```
achievements/
├── README.md           ← master index
├── work/
│   └── <project>/
│       └── LOG.md      ← per-task work log
└── personal/
    └── <project>/
        └── PROFILE.md  ← resume-style project summary
```

## Reports

| Command | Output |
|---------|--------|
| "weekly report" | Last 7 days, work entries only |
| "monthly report" | Current month |
| "quarterly / Q1–Q4 report" | Quarter |
| "half-yearly / H1/H2 report" | 6 months |
| "yearly report" | Full year |
| "resume bullets" | All time, work + personal |
