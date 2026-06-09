# 🤖 Job Search Agent Overview

An AI-powered job search agent that autonomously finds job postings, analyzes them against my resume, scores the match, identifies red flags, and automatically logs strong opportunities to a Google Sheet — built with Python and the Anthropic Claude API.

---

## What It Does

### Autonomous Mode
1. **Searches** Google Jobs via SerpAPI for roles matching my criteria
2. **Fast scans** each role using the job description returned by SerpAPI
3. **Scores** the match against my resume using Claude
4. **Deep dives** on strong matches (70+) — fetches the full job posting page for enriched analysis
5. **Decides** autonomously — logs only roles that meet the score threshold
6. **Logs** to Google Sheets — date, title, company, score, URL, fit notes, red flags, bottom line, date posted, and compensation range

### Manual Mode
- Paste any job posting URL directly and get instant analysis
- Batch mode — paste multiple URLs at once and process them all in one run

---

## Example Output

```
🔍 Searching for: Customer Success Director
📍 Location: San Francisco

✅ Found 10 roles. Analyzing each one...

[1/10] Director of Customer Success at Salesforce
Match Score: 82/100
Date Posted: June 2026
Compensation: $180,000 - $220,000
Notes: Strong alignment with 15+ years in SaaS Customer Success leadership...
Red Flags: Large org may limit autonomy compared to startup experience...
Bottom Line: Strong fit — apply within the week.

⭐ Strong match! Fetching full posting for richer data...
✅ Full posting fetched — doing enriched analysis...
Logging to Google Sheet...

📊 SEARCH SUMMARY
#    Score    Company                   Title
═══════════════════════════════════════════════════════════════════════════
1    82/100   Salesforce                Director of Customer Success    STRONG MATCH
2    78/100   DroneDeploy               Director of Customer Success    STRONG MATCH
3    61/100   Punch Agency              Director Customer Success
...

4 of 10 roles logged to your Google Sheet
```

---

## Tech Stack

| Tool | Purpose |
|------|---------|
| Python | Core language |
| Anthropic Claude API | Job analysis, match scoring, red flag detection |
| SerpAPI (Google Jobs) | Autonomous job search across multiple boards |
| BeautifulSoup | Web scraping full job postings for enriched analysis |
| gspread | Google Sheets integration |
| Google Service Account | Sheets authentication |
| python-dotenv | Secure API key management |
| GitHub Actions | Scheduled daily runs at 8am Pacific |

---

## Architecture

```
Daily trigger (GitHub Actions 8am PT)
    ↓
SerpAPI fetches 10 matching roles from Google Jobs
    ↓
Claude fast-scans each role from description
    ↓
Score < 70 → skip
Score ≥ 70 → fetch full job posting URL
    ↓
Claude deep-dives the full posting
    ↓
Log enriched result to Google Sheets
```

This two-pass design keeps the agent fast on low-quality matches while ensuring strong matches get the richest possible analysis.

---

## Project Structure

```
job-search-agent/
├── job_finder.py        # Autonomous mode — searches SerpAPI and runs full pipeline
├── job_matcher.py       # Manual/batch mode — analyzes URLs you paste in
├── job_reader.py        # Standalone job description analyzer
├── search_config.py     # Search criteria — edit to change keywords or location
├── first_call.py        # Initial Claude API proof of concept
├── .github/workflows/
│   └── job_search.yml   # GitHub Actions scheduled workflow
├── resume.txt           # Resume (local only, not tracked)
├── credentials.json     # Google credentials (local only, not tracked)
├── .env                 # API keys (local only, not tracked)
└── .gitignore           # Keeps sensitive files off GitHub
```

---

## Setup

### 1. Clone the repo
```bash
git clone https://github.com/slacy77/job-search-agent
cd job-search-agent
```

### 2. Install dependencies
```bash
pip3 install anthropic python-dotenv requests beautifulsoup4 gspread google-auth google-search-results
```

### 3. Add your credentials
Create a `.env` file:
```
ANTHROPIC_API_KEY=your_anthropic_key_here
SERPAPI_KEY=your_serpapi_key_here
```

Add your Google Service Account `credentials.json` to the project folder.

### 4. Add your resume
Save your resume as `resume.txt` in the project folder.

### 5. Configure your search
Edit `search_config.py`:
```python
SEARCH_CONFIG = {
    "keywords": "Customer Success Director",
    "location": "San Francisco",
    "country": "us",
    "max_results": 10,
    "min_match_score": 70
}
```

### 6. Run the agent

**Autonomous mode:**
```bash
python3 job_finder.py
```

**Manual/batch mode:**
```bash
python3 job_matcher.py
```

---

## Google Sheet Output

Every strong match (score 70+) is automatically logged with:

| Date | Job Title | Company | Match Score | Job URL | Key Notes | Red Flags | Bottom Line | Date Posted | Compensation |
|------|-----------|---------|-------------|---------|-----------|-----------|-------------|-------------|--------------|

---

## Roadmap

- [x] Single job URL analysis
- [x] Resume match scoring
- [x] Red flag detection
- [x] Google Sheets logging
- [x] Batch mode — analyze multiple URLs at once
- [x] Autonomous job finder via SerpAPI (Google Jobs)
- [x] Two-pass analysis — fast scan + deep dive on strong matches
- [x] Fully automated daily runs via GitHub Actions
- [x] Compensation and date posted extraction
- [ ] Duplicate detection — prevent same role being logged twice
- [ ] Resume rewriter — tailors resume bullets to match a specific role
- [ ] Cover letter generator — drafts a personalized cover letter for strong matches
- [ ] Multi-keyword search — rotate through multiple job titles automatically

---

## Why I Built This

I built this agent during my job search to solve a real problem — manually reading and evaluating job postings is time-consuming and easy to do inconsistently. This agent gives me a structured, repeatable analysis of every role in seconds, automatically tracks the ones worth pursuing, and runs every morning before I wake up.

It's also a deliberate learning project: I wanted hands-on experience building a real AI agent from scratch — including API integration, web scraping, autonomous decision logic, two-pass enrichment architecture, and cloud scheduling via GitHub Actions.

---

*Built with the Anthropic Claude API*
