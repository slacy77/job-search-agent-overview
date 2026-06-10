# Job Search Agent — Project Overview

An AI-powered job search agent I built from scratch during parental leave to automate and improve my own job search. It finds roles, evaluates them against my background, generates application materials, and runs every morning without me touching it.

This page is a plain-English walkthrough of what I built, how it works, and what I learned. The code lives in a private repo to protect credentials and personal data.

---

## The Problem I Was Solving

Job searching manually is slow and inconsistent. Reading 10 postings, evaluating fit, tailoring a resume, drafting a cover letter — done properly it takes hours per day. I wanted to automate the repetitive parts so I could focus on the parts that actually require a human: conversations, relationships, and judgment calls.

I also wanted to build something real with the Anthropic Claude API rather than just take courses about it.

---

## What It Does

### Every morning, automatically:
1. Searches Google Jobs for Customer Success leadership roles in my target market
2. Fast-scans each role against my resume and scores the match out of 100
3. Skips anything below 70 — and skips roles already in my tracker
4. Deep-dives on strong matches by fetching the full job posting page
5. Generates a tailored cover letter as a starting draft
6. Rewrites my resume bullets to surface the most relevant experience for that role
7. Logs everything to a Google Sheet — score, fit notes, red flags, compensation, date posted

### On demand:
- Paste any job URL and get instant analysis
- Batch mode — paste 10 URLs at once and process them all

---

## How It Works

```
GitHub Actions triggers at 8am Pacific
        ↓
SerpAPI searches Google Jobs for matching roles
        ↓
Claude fast-scans each role from the job description
        ↓
Score < 70 → skip
Already in tracker → skip (duplicate detection)
        ↓
Score ≥ 70 + new role → fetch full job posting page
        ↓
Claude deep-dives the full posting (enriched analysis)
        ↓
Generate tailored cover letter → save to file
Rewrite resume bullets → save to file
Log full analysis to Google Sheets
```

The two-pass design keeps things fast — Claude only does the expensive deep-dive on roles that actually clear the bar.

---

## What the Output Looks Like

**In the terminal:**
```
🔍 Searching for: Customer Success Director
📍 Location: San Francisco

✅ Found 10 roles. Analyzing each one...

[3/10] Director of Customer Success at DroneDeploy
Match Score: 78/100
Compensation: $170,000 - $200,000
Date Posted: June 2026
Notes: Strong alignment with CS leadership background and team-building experience...
Red Flags: Series C stage with rapid headcount growth may create hiring pressure...
Bottom Line: Strong fit — early-stage enough to have real impact, funded enough to be stable.

⭐ Strong match! Fetching full posting for richer data...
✅ Logged to Google Sheet
📄 Cover letter saved
📄 Rewritten resume saved
```

**In Google Sheets:**

| Date | Job Title | Company | Score | URL | Key Notes | Red Flags | Bottom Line | Date Posted | Compensation |
|------|-----------|---------|-------|-----|-----------|-----------|-------------|-------------|--------------|
| 2026-06-10 | Director of CS | DroneDeploy | 78 | ... | Strong alignment... | Series C hiring pressure... | Strong fit... | June 2026 | $170k-$200k |

---

## Tech Stack

| Tool | Purpose |
|------|---------|
| Python | Core language |
| Anthropic Claude API | All AI reasoning — scoring, analysis, cover letters, resume rewriting |
| SerpAPI | Searches Google Jobs aggregating LinkedIn, Indeed, and others |
| BeautifulSoup | Scrapes full job posting pages for enriched analysis |
| gspread + Google Service Account | Reads and writes to Google Sheets |
| python-dotenv | Keeps API keys out of code |
| GitHub Actions | Runs the agent on a schedule every morning |

---

## What Makes This a Real Agent

A basic script would take a URL and summarize it. This agent:

- **Plans** — decides which roles are worth deeper analysis
- **Uses tools** — search API, web scraper, Google Sheets, file system
- **Has memory** — checks its own history to avoid duplicate work
- **Makes decisions** — score below threshold? skip. already logged? skip. strong match? enrich, generate, log
- **Acts autonomously** — the whole pipeline runs without human input at any step

The cover letter and resume rewriter don't fabricate anything. They help surface and frame real experience in the language each specific role responds to — the same thing a career coach does, just faster.

---

## What I Built It With

No prior local dev setup. No CS degree. Built over roughly two days while on parental leave, starting from scratch with Python, VS Code, and the Anthropic API docs.

The skills I developed hands-on:
- Python scripting and API integration
- Prompt engineering for structured JSON output
- Web scraping with BeautifulSoup
- Google Sheets API via service account authentication
- GitHub Actions for cloud scheduling
- Secure credential management (.env, GitHub Secrets)
- Git version control

---

## What I'd Build Next

- **Multi-keyword search** — rotate through multiple job titles in one run
- **Email digest** — morning summary of new strong matches delivered to inbox
- **Application tracker** — log when I apply, follow up reminders, interview stage tracking

---

## Why This Matters for CS Leadership Roles

Customer Success is increasingly an AI-augmented function. The CS leaders who will be most effective in the next few years aren't just great at relationships and retention — they understand how to build systems, automate workflows, and use AI tools to scale their team's impact.

Building this agent wasn't just a learning exercise. It's a demonstration of how I think about problems: find the repetitive parts, automate them, and free up human time for the work that actually requires judgment.

---

*Built with the Anthropic Claude API · [GitHub Profile](https://github.com/slacy77/slacy77)*
