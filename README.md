# Veritasium YouTube Analytics Dashboard

A full-stack analytics project that pulls real data from the YouTube Data API v3, processes it in Google Colab, and visualizes it in a Tableau-style interactive dashboard built entirely in HTML, CSS, and JavaScript.

Built as a portfolio project to demonstrate product analytics skills across data extraction, metric design, funnel analysis, and insight communication.

---

## What this project does

It analyzes 200 videos from the Veritasium YouTube channel (@veritasium, 20.7M subscribers, 4.23B views) and answers real product analytics questions:

- Where do viewers drop off in the engagement funnel?
- Which content format performs best per view and per dollar?
- When should you post to maximize first-48-hour reach?
- What does the channel's monetization actually look like at different RPM assumptions?
- Which metrics are real API data versus estimates, and why does that distinction matter?

---

## Project structure

```
veritasium-analytics/
├── Veritasium_YouTube_Analytics.ipynb   # Google Colab notebook for data extraction
├── dashboard/
│   └── veritasium_tableau.html          # Full interactive dashboard (open in browser)
├── data/                                # CSVs output by the notebook (not tracked in git)
│   ├── channel_summary.csv
│   ├── videos_full.csv
│   ├── funnel_data.csv
│   ├── posting_heatmap.csv
│   ├── category_breakdown.csv
│   ├── monthly_growth.csv
│   ├── comments_sentiment.csv
│   └── top20_leaderboard.csv
└── README.md
```

---

## How to run it yourself

### Step 1: Get a YouTube Data API key

1. Go to [console.cloud.google.com](https://console.cloud.google.com)
2. Create a new project
3. Go to APIs and Services, then Library
4. Search for YouTube Data API v3 and enable it
5. Go to Credentials, click Create Credentials, then API Key
6. Copy your key

The free tier gives you 10,000 units per day. This notebook uses roughly 400 to 600 units per run, so you will not hit the limit.

### Step 2: Run the Colab notebook

1. Open `Veritasium_YouTube_Analytics.ipynb` in [Google Colab](https://colab.research.google.com)
2. Paste your API key in Cell 2 where it says `PASTE_YOUR_NEW_API_KEY_HERE`
3. Run all cells (Runtime > Run all)
4. The notebook will extract all data and download a ZIP file of CSVs at the end

To analyze a different channel, change `CHANNEL_HANDLE = '@veritasium'` to any YouTube handle.

### Step 3: Open the dashboard

Open `dashboard/veritasium_tableau.html` in any modern browser. No server needed, no dependencies to install. It runs entirely in the browser.

---

## Dashboard sections

| Sheet | What it shows | Data source |
|---|---|---|
| Overview | Core channel KPIs, top videos, engagement distribution | Real API data |
| Growth and Uploads | Monthly upload cadence, cumulative views, posting heatmap | Real API data |
| Engagement Funnel | Impressions through subscriber gains, drop-off at each stage | Mixed (views real, impressions estimated) |
| Retention and Cohorts | Watch time by format, cohort decay model | Estimated using industry benchmarks |
| Monetization | Revenue estimates by video and format, RPM sensitivity analysis | Estimated (views real, RPM assumed) |
| Strategy | Format performance matrix, sentiment analysis, recommendations | Real API data |
| Case Study | Written analysis of what the data actually reveals | Analysis |
| Estimation Notes | Full methodology for every estimated metric | Documentation |

---

## Being honest about what is real vs estimated

Every metric in this project is labeled. The YouTube Data API gives you view counts, like counts, comment counts, video duration, and publish timestamps. Everything else requires OAuth login as the channel owner, which is not possible when analyzing someone else's channel.

**Real data from the API:**
- View counts, like counts, comment counts
- Video duration and publish timestamps
- Channel subscriber count and total view count
- Comment text (used for sentiment analysis)

**Estimated using industry benchmarks:**
- Impressions (calculated as views divided by assumed 4% CTR)
- Revenue (calculated as views per thousand times $2.50 RPM, deliberately conservative)
- Watch time and retention rates (industry benchmark percentages applied to real durations)
- Geographic distribution (standard patterns for English-language education channels)

The Estimation Notes sheet in the dashboard explains every formula, the source of every benchmark, and what the margin of error is.

---

## Tech stack

| Tool | Purpose |
|---|---|
| YouTube Data API v3 | Data extraction |
| Google Colab + Python | Data processing and CSV export |
| pandas, isodate, tqdm | Data manipulation |
| Plotly.js | All charts and visualizations |
| HTML, CSS, JavaScript | Dashboard interface |
| Bai Jamjuree + JetBrains Mono | Typography |

The dashboard has no build step and no framework dependencies. It is a single HTML file you can open directly.

---

## Key findings

A few things that came out of the data that were worth noting:

Shorts are significantly underused. The 29 Shorts in the dataset average 16 million views each. Regular short-form videos (1 to 5 minutes) average 8.9 million. That is a 79% difference per video, and Shorts make up only 14.5% of uploads.

Tuesday morning UTC is consistently the best posting window based on real publish timestamps matched against real view counts.

The like-to-comment ratio is 24 to 1. The audience appreciates the content deeply but is largely passive. Ending videos with a direct question would likely change this.

Revenue estimates are conservative by design. The $6.64M lifetime figure uses $2.50 RPM. Education channels typically earn $4 to $8 RPM. At $5 RPM the figure becomes $13.3M, before factoring in sponsorships which the public API cannot see at all.

---

## What I learned building this

Designing for data honesty is harder than it sounds. The instinct when building a portfolio analytics project is to make everything look impressive and definitive. The harder and more useful thing is to be precise about what you actually know versus what you are inferring, and to build that distinction into the interface itself so a viewer can see it at a glance.

Product analytics is not just about charts. The Estimation Notes sheet and the written Case Study took as long to produce as all the charts combined. The ability to explain your methodology and write clearly about what the data does and does not tell you is the actual skill.

---

## License

MIT. Use it, fork it, change the channel handle to analyze someone else.

---

## Contact

If you have questions about the methodology or want to adapt this for a different data source, feel free to open an issue.
