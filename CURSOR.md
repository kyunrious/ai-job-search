# Using AI Job Search with Cursor

This fork keeps the original Claude Code workflows and adds Cursor-facing instructions. Cursor should read `.cursor/rules/ai-job-search.mdc` automatically when this repository is open.

## Quick Start

1. Open this repository in Cursor.
2. Install the non-AI prerequisites from `SETUP.md`: Python 3.10+, Bun, and a LaTeX distribution with `lualatex` and `xelatex`.
3. Install job search CLI dependencies:

```bash
cd .agents/skills/jobbank-search/cli && bun install && cd ../../../..
cd .agents/skills/jobdanmark-search/cli && bun install && cd ../../../..
cd .agents/skills/jobindex-search/cli && bun install && cd ../../../..
cd .agents/skills/jobnet-search/cli && bun install && cd ../../../..
```

4. In Cursor chat, use the same workflow phrases as the Claude version:

```text
/setup
/scrape
/apply https://example.com/job-posting
/expand
/upskill https://example.com/job-posting
/reset profile
```

If Cursor does not treat these as native slash commands, type: "Follow `.claude/commands/apply.md` for this job posting: <url-or-text>".

## How Compatibility Works

The `.claude/` directory remains the workflow source of truth:

- `.claude/commands/setup.md` - profile onboarding
- `.claude/commands/apply.md` - fit evaluation, CV, cover letter, review, PDF verification
- `.claude/commands/expand.md` - enrich profile from public sources and documents
- `.claude/commands/reset.md` - reset profile or documents after confirmation
- `.claude/skills/job-scraper/SKILL.md` - job search and deduplication
- `.claude/skills/upskill/SKILL.md` - skill gap analysis and learning plan

The Cursor rule maps Claude Code tool names to Cursor capabilities. For example, "Agent tool" means a Cursor subagent if available, and "Read tool" means Cursor file or PDF reading.

## Important Differences from Claude Code

- Cursor may not register `/setup`, `/apply`, or `/scrape` as actual slash commands. The `.cursor` rule tells Cursor to interpret those strings as workflow requests.
- Claude Code permission settings in `.claude/settings.local.json` do not configure Cursor. Cursor will ask for tool approval according to its own settings.
- Reviewer-agent behavior depends on Cursor subagent availability. If a subagent is not available, run the reviewer as a separate pass in the same chat.
- Application materials should mention the AI coding tool the candidate actually used. Do not force "Claude Code" if the truthful tool is Cursor.

## Recommended Cursor Prompts

Setup from documents:

```text
Run /setup. Use Path A and read the documents folder.
```

Apply to a job:

```text
Run /apply for this posting: https://example.com/job-posting
Evaluate fit first, then ask before drafting.
```

Search for jobs:

```text
Run /scrape broad. Use the configured search queries and deduplicate against job_scraper/seen_jobs.json and job_search_tracker.csv.
```

Use LinkedIn in any market:

```text
Use .agents/skills/linkedin-search/SKILL.md to search for product marketing roles in San Francisco, posted in the last 14 days.
```
