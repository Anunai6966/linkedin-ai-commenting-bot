# Changelog

## v1.0.0 — Initial Build

### What was built
- Dual-chain n8n workflow for autonomous LinkedIn commenting
- Top chain: post discovery via Apify scraper with URN-based deduplication
- Bottom chain: AI comment generation via Gemini 2.5 Flash + Unipile API posting
- Airtable as the central database for profiles, posts, and brand guidelines
- Full status tracking: New → Responded / Failed

### Tools integrated
- n8n Cloud — workflow orchestration
- Apify (Profile Posts Scraper for LinkedIn) — post discovery
- Airtable — database and status tracking
- Google Gemini 2.5 Flash — AI comment generation
- Unipile — LinkedIn API bridge for posting comments

### Key design decisions
- URN-based deduplication prevents re-commenting on the same post
- Dual-chain separation keeps discovery and commenting independent
- Brand table allows per-client tone of voice injection
- Loop Over Items handles multi-profile scraping in a single top chain run

### Fixes and improvements applied post-initial build
- Fixed hardcoded username in Apify Input JSON → made dynamic with `$json.Name`
- Fixed `[object Object]` URN issue → changed to `$json.urn.activity_urn`
- Added `alwaysOutputData: true` to Search records1 to prevent workflow stopping on no match
- Added error handling: HTTP Request set to Continue on Error
- Added IF1 node to route success/failure after Unipile call
- Added Update record1 (Failed status) for failed Unipile calls
- Updated Search records2 filter from `{Reply}=""` to `AND({Reply}="", {Status}="New")`
- Added Failed option to Airtable Status field
- Changed Edit Fields field names from Expression mode to Fixed mode
- Increased Apify limit from 1 to 2 posts per profile per run
- Fixed `.item` reference error in Edit Fields → changed to `.first()`
- Set Schedule Trigger1 to every 2 hours at minute :23 (natural offset)
- Locked Gemini model to `gemini-2.5-flash` explicitly
- Set workflow timezone to Asia/Kolkata (IST)
