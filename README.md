# 📊 YouTube Trend Analytics Automation (n8n)

An end-to-end, no-code automation built in **n8n** that tracks trending YouTube videos for any topic, calculates real trend velocity, and delivers a ranked report straight to your inbox — fully hands-off.

![n8n](https://img.shields.io/badge/n8n-workflow-orange)
![Automation](https://img.shields.io/badge/type-automation-blue)
![Status](https://img.shields.io/badge/status-active-brightgreen)

---

## 🎥 Demo

<!-- Replace the line below with your actual demo video/GIF once uploaded -->
[![Watch the demo](demo/thumbnail.png)](demo/demo-video.mp4)

*(See [Adding a Video Demo](#-adding-a-video-demo-to-this-repo) below for exact upload steps.)*

---

## 🚀 What This Project Does

Manually checking YouTube for trending content in a niche doesn't scale — raw view counts are misleading, since a 3-year-old video with 1M views isn't "trending," but a 5-day-old video with 50K views might be.

This workflow automates the entire process:

- 📥 Accepts a search topic (via a form or hardcoded config)
- 🔎 Pulls live YouTube search data through a RapidAPI endpoint
- 🧮 Calculates **views-per-day** — a fair trend-velocity metric instead of raw views
- 🧹 Filters out low-performing/noise results automatically
- 🗂️ Logs every result to Google Sheets for historical tracking
- 📧 Emails a clean, ranked HTML report — no manual work required
- ⚠️ Includes dedicated error-handling paths that alert you if the API fails, hits quota limits, or returns no results

---

## 🧱 Architecture

```
[Schedule / Form Trigger]
        ↓
[Set: Config] → search query, thresholds, report email
        ↓
[HTTP Request: Search Videos] → RapidAPI YouTube search endpoint (with retries)
        ↓
[IF: Has Results?] → catches empty/failed responses
        ↓
[Split Out: Contents] → breaks response into individual items
        ↓
[IF: Is Video?] → filters out non-video results (channels, etc.)
        ↓
[Set: Extract Basic Fields] → title, channel, views, published date, URL
        ↓
[Code: Clean & Calculate Metrics] → parses dates, computes views/day
        ↓
   ┌────────────────────┬─────────────────────┐
   ↓                     ↓
[Google Sheets: Log]  [IF: Top Performer?]
                          ↓
                    [Sort: By Views/Day]
                          ↓
                 [Aggregate: Build Report Array]
                          ↓
                  [Code: Build HTML Report]
                          ↓
                  [Send Email: Trend Report]

Error branches (Search fails / No results / API failure)
        ↓
  [Send Email: Error Alert]
```

---

## 🛠️ Tech Stack

| Layer | Tool |
|---|---|
| Workflow orchestration | [n8n](https://n8n.io) |
| Data source | [RapidAPI](https://rapidapi.com) — YouTube search API |
| Data transformation | JavaScript (n8n Code nodes) |
| Data storage / history | Google Sheets |
| Report delivery | Gmail API |
| Error monitoring | n8n error-output branches + email alerts |

---

## 📁 Repository Structure

```
youtube-trend-automation/
├── workflows/
│   └── youtube_trend_automation.json   # Import this directly into n8n
├── demo/
│   ├── demo-video.mp4                  # Screen recording of the workflow running
│   └── thumbnail.png                   # Thumbnail image for the demo video
├── screenshots/
│   └── workflow-canvas.png             # Screenshot of the full n8n canvas
└── README.md
```

---

## ⚙️ Setup & Installation

### 1. Prerequisites
- An [n8n](https://n8n.io) instance (cloud, desktop, or self-hosted)
- A free [RapidAPI](https://rapidapi.com) account subscribed to a YouTube search API
- A Google account (for Sheets logging)
- A Gmail account (for sending reports)

### 2. Import the workflow
1. Download `workflows/youtube_trend_automation.json` from this repo
2. In n8n: **Workflows → Import from File**
3. Select the downloaded JSON

### 3. Configure credentials
- Open the **HTTP Request** nodes → paste your RapidAPI key and host into the headers
- Open the **Google Sheets** node → connect your Google account and paste your Sheet ID
- Open the **Gmail** node → connect your Gmail account

### 4. Set your parameters
In the **Set: Config** node, edit:
- `searchQuery` — the topic/niche to track
- `minViewsPerDay` — the threshold for "trending"
- `reportEmail` — where the report should be sent

### 5. Test, then activate
- Run the workflow manually first to confirm everything connects properly
- Once a clean run completes, activate the schedule trigger

---

## 📈 Example Output

The final email report includes a ranked table of trending videos:

| Title | Channel | Views | Views/Day |
|---|---|---|---|
| Example Video Title | Example Channel | 830,040 | 3,952 |
| Another Trending Video | Another Channel | 1,061,695 | 2,910 |

*(Actual numbers will reflect live data at the time the workflow runs.)*

---

## 🎬 Adding a Video Demo to This Repo

GitHub doesn't allow embedding a playable video directly in a README the way YouTube does, but here are the two solid options:

**Option A — Host on GitHub (short clips only, works well for <10MB files)**
1. Create a folder in your repo: `demo/`
2. Drag your `.mp4` file directly into that folder in the GitHub web UI (Add file → Upload files)
3. In your README, add:
   ```markdown
   https://github.com/your-username/your-repo/assets/your-video-file.mp4
   ```
   GitHub automatically renders uploaded video files as an inline playable player when linked this way inside a README.

**Option B — Host on YouTube/Loom, embed a clickable thumbnail (recommended for longer demos)**
1. Upload your screen recording to YouTube (unlisted is fine) or Loom
2. Take a screenshot of the video as a thumbnail, save it as `demo/thumbnail.png`
3. Add this to your README:
   ```markdown
   [![Watch the demo](demo/thumbnail.png)](https://youtu.be/your-video-id)
   ```
   Clicking the thumbnail image will take viewers to the full video.

**Recommended for portfolio purposes:** Option B — a Loom or YouTube demo also lets you narrate what's happening, which is much more persuasive to recruiters/clients than a silent clip.

---

## 💡 Possible Extensions

- Swap Google Sheets for a proper database (Postgres/Airtable) for larger datasets
- Add sentiment analysis on video titles/descriptions using an NLP node
- Trigger via a public-facing **Form Trigger** so clients can request reports on-demand
- Add Slack/Discord delivery as an alternative to email
- Track the same set of channels over time to measure growth, not just one-off searches

---

## 📬 Contact

Built by **[Your Name]** — open to freelance/contract automation work.
- Fiverr: [your gig link]
- LinkedIn: [your profile link]
- Email: [your email]

---

## 📄 License

This project is open for reference and personal/commercial adaptation. Attribution appreciated but not required.
