# LinkedIn AI Commenting Bot

An autonomous LinkedIn engagement system that discovers posts from target profiles, generates contextual AI comments using Gemini, and posts them directly to LinkedIn — fully automated, zero manual intervention.

Built with n8n, Apify, Airtable, Google Gemini 2.5 Flash, and Unipile.

---

## How it works

The system runs as two independent n8n workflow chains:

**Chain 1 — Post Discovery (runs daily)**
Scrapes recent posts from a list of LinkedIn profiles using Apify, checks for duplicates via URN matching in Airtable, and saves new posts with Status = `New`.

**Chain 2 — AI Comment Generation (runs every 2 hours)**
Picks up posts with Status = `New`, fetches the brand tone of voice from Airtable, generates a contextual comment using Gemini 2.5 Flash, posts it to LinkedIn via Unipile API, and updates the record to `Responded` — or `Failed` if the post was inaccessible.

![Workflow Architecture](architecture.png)

---

## Tech Stack

| Tool | Purpose |
|---|---|
| n8n Cloud | Workflow orchestration |
| Apify | LinkedIn profile post scraper |
| Airtable | Database — profiles, posts, brand voice |
| Google Gemini 2.5 Flash | AI comment generation |
| Unipile | LinkedIn API bridge for posting comments |

---

## Features

- Fully automated end-to-end — no human input after setup
- URN-based deduplication prevents re-commenting on the same post
- Brand voice injection — per-client tone of voice from Airtable
- Error handling — failed Unipile calls marked as `Failed`, not silently dropped
- Multi-profile support — loops through any number of LinkedIn profiles
- ACA commenting framework — Acknowledge, Context, Action + Question
- Schedule-based triggering with natural time offsets

---

## Project Structure
```
linkedin-ai-commenting-bot/
├── workflow.json        # n8n workflow export (credentials replaced with placeholders)
├── airtable_schema.md   # Airtable base structure — tables, fields, field types
├── system_prompt.md     # Full AI system prompt used for comment generation
├── .env.example         # Environment variable template
├── CHANGELOG.md         # Build log and all fixes applied
├── architecture.png     # n8n workflow canvas screenshot
└── README.md
```

---

## Setup Guide

### 1. Airtable
- Create a new Airtable base called `Linkedin AutoComment`
- Create 3 tables: `Profiles`, `Posts`, `Brand` — refer to `airtable_schema.md` for full field structure
- Add your target LinkedIn usernames to the Profiles table
- Add your brand tone of voice to the Brand table

### 2. Apify
- Sign up at [apify.com](https://apify.com)
- Find the actor: **Profile Posts Scraper for LinkedIn [No Cookies]** by apimaestro
- Connect your Apify account to n8n via OAuth

### 3. Unipile
- Sign up at [unipile.com](https://unipile.com)
- Connect your LinkedIn account inside Unipile
- Note your DSN, API key, and account ID

### 4. Google Gemini
- Get an API key from [Google AI Studio](https://aistudio.google.com)
- Add it as a Google Gemini credential in n8n

### 5. n8n
- Import `workflow.json` into your n8n instance
- Replace all placeholder credentials with your actual credentials
- Update the Unipile DSN and account ID in the HTTP Request node
- Activate both schedule triggers
- Run the top chain manually first to populate Airtable with posts
- Then run the bottom chain to test comment generation

---

## Airtable Status Flow
```
New → Responded   (comment posted successfully)
New → Failed      (Unipile returned an error — post inaccessible)
```

Failed posts are not retried automatically. Review them in Airtable and delete or manually handle.

---

## AI Comment Framework

Comments follow the **ACA + Warm Question** structure:

- **Acknowledge** — affirm the core insight of the post
- **Context** — add a reframe or perspective that sharpens the idea
- **Action** — leave a practical takeaway
- **Question** — end with a genuine peer-to-peer question

Full prompt available in `system_prompt.md`.

---

## Cost Estimate (Monthly)

| Tool | Cost |
|---|---|
| n8n Cloud | $20/mo |
| Unipile | $49/mo |
| Apify | ~$3/mo |
| Gemini API | Free tier |
| Airtable | Free tier |
| **Total** | **~$72/mo** |

---

## Built By

**Anunai Sai** — Final year B.Tech CS undergrad  
Interests: ML, AI Automation, n8n, Computer Vision, NLP  
[LinkedIn](https://linkedin.com/in/your-profile) • [GitHub](https://github.com/Anunai6966)
