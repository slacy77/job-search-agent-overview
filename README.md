# Job Search Agent — Project Overview

An AI-powered job search agent I built from scratch during parental leave to automate and improve my own job search. It finds roles across multiple job titles, evaluates them against my background, generates application materials, and runs every morning without me touching it.

This page is a plain-English walkthrough of what I built, how it works, and what I learned. The code lives in a private repo to protect credentials and personal data.

---

## The Problem I Was Solving

Job searching manually is slow and inconsistent. Reading 10 postings, evaluating fit, tailoring a resume, drafting a cover letter — done properly it takes hours per day. I wanted to automate the repetitive parts so I could focus on the parts that actually require a human: conversations, relationships, and judgment calls.

I also wanted to build something real with the Anthropic Claude API rather than just take courses about it.

---

## What It Does

### Every morning, automatically:
1. Searches Google Jobs across 4 job titles simultaneously — Director of Customer Success, Director of Technical Account Management, VP of Customer Success, Director of Technical Customer Success
2. Deduplicates results across all keyword searches so no role gets analyzed twice
3. Fast-scans each role against my resume and scores the match out of 100
4. Skips anything below 70 — and skips roles already in my tracker (it has memory across runs)
5. Deep-dives on strong matches by fetching the full job posting page for richer analysis
6. Generates a tailored cover letter as a starting draft
7. Rewrites my resume bullets with a side-by-side comparison — original, rewritten, and the reasoning behind each change
8. Logs everything to a Google Sheet — score, fit notes, red flags, compensation, date posted

### On demand:
- Paste any job URL and get instant analysis
- Batch mode — paste multiple URLs at once and process them all

---

## How It Works

```
GitHub Actions triggers at 8am Pacific
        ↓
SerpAPI searches Google Jobs across 4 job titles
Deduplicates results across all keyword searches
        ↓
Claude fast-scans each unique role from the job description
        ↓
Score < 70 → skip
Already in tracker → skip (duplicate detection)
        ↓
Score ≥ 70 + new role → fetch full job posting page
        ↓
Claude deep-dives the full posting (enriched analysis)
        ↓
Generate tailored cover letter → save to file
Rewrite resume bullets (side-by-side with reasoning) → save to file
Log full analysis to Google Sheets
```

The two-pass design keeps things fast — Claude only does the expensive deep-dive on roles that actually clear the bar.

---

## What the Output Looks Like

**In the terminal:**
```
📍 Location: San Francisco Bay Area
🔍 Searching 4 keyword(s)...

  → Director of Customer Success
  → Director of Technical Account Management
  → VP of Customer Success
  → Director of Technical Customer Success

✅ Found 18 unique roles. Analyzing each one...

[3/18] Director of Customer Success at DroneDeploy
Match Score: 78/100
Compensation: $170,000 - $200,000
Date Posted: June 2026
Notes: Strong alignment with CS leadership background...
Red Flags: Series C hiring pressure may impact culture...
Bottom Line: Strong fit — apply within the week.

⭐ Strong match! Fetching full posting for richer data...
✅ Logged to Google Sheet
📄 Cover letter saved
📄 Rewritten resume saved
```

**Resume rewriter output (side-by-side):**
```
ORIGINAL: Led team of 12 CSMs across SMB and Mid-Market segments
REWRITTEN: Built and scaled a 12-person CSM team across SMB and Mid-Market, driving retention initiatives and owning GRR/NRR outcomes
REASON: Added outcome-focused language matching the role's emphasis on retention metrics and revenue ownership
```

**In Google Sheets:**

| Date | Job Title | Company | Score | Key Notes | Red Flags | Bottom Line | Compensation |
|------|-----------|---------|-------|-----------|-----------|-------------|--------------|
| 2026-06-10 | Director of CS | DroneDeploy | 78 | Strong alignment... | Series C pressure... | Strong fit... | $170k-$200k |

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

The cover letter and resume rewriter don't fabricate anything. They help surface and frame real experience in the language each specific role responds to — the same thing a career coach does, just faster. Every suggested change includes the reasoning behind it so I can decide what to keep.

---

## What I Built It With

No prior local dev setup. No CS degree. Built over roughly two days while on parental leave with a newborn and a toddler, starting from scratch with Python, VS Code, and the Anthropic API docs.

The skills I developed hands-on:
- Python scripting and API integration
- Prompt engineering for structured JSON output
- Web scraping with BeautifulSoup
- Google Sheets API via service account authentication
- GitHub Actions for cloud scheduling
- Secure credential management (.env, GitHub Secrets)
- Git version control
- Multi-source data aggregation and deduplication

---

## What I'd Build Next

- **Email digest** — morning summary of new strong matches delivered to inbox
- **Application tracker** — log when I apply, follow up reminders, interview stage tracking
- **Salary benchmarking** — cross-reference compensation data across similar roles

---

## Why This Matters for CS Leadership Roles

Customer Success is increasingly an AI-augmented function. The CS leaders who will be most effective in the next few years aren't just great at relationships and retention — they understand how to build systems, automate workflows, and use AI tools to scale their team's impact.

Building this agent wasn't just a learning exercise. It's a demonstration of how I think about problems: find the repetitive parts, automate them, and free up human time for the work that actually requires judgment.

---

*Built with the Anthropic Claude API · [GitHub Profile](https://github.com/slacy77/slacy77)*
