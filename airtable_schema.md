# Airtable Schema

Base name: **Linkedin AutoComment**

---

## Table 1 — Profiles

Stores the LinkedIn usernames of people whose posts you want to monitor and comment on.

| Field Name | Field Type | Notes |
|---|---|---|
| Name | Single line text | LinkedIn username (e.g. `ankitaevara`) |

---

## Table 2 — Posts

Stores every discovered LinkedIn post and tracks its commenting status.

| Field Name | Field Type | Notes |
|---|---|---|
| Profile | Single line text | LinkedIn username of the post author |
| Post | Long text | Full text content of the LinkedIn post |
| Reply | Long text | AI-generated comment (filled by bottom chain) |
| Status | Single select | Options: `New`, `Responded`, `Failed` |
| PostURL | URL | Full LinkedIn post URL |
| URN | Single line text | LinkedIn post activity URN (unique ID) |
| Go to Post | Formula / Button | Quick link to open the post on LinkedIn |

---

## Table 3 — Brand

Stores the tone of voice and brand guidelines used by the AI to generate comments.

| Field Name | Field Type | Notes |
|---|---|---|
| Tone of Voice | Long text | Full brand voice guidelines fed into the AI prompt |

---

## Notes

- The `URN` field in Posts is used for deduplication — the top chain checks this before saving a new post
- The `Status` field drives the bottom chain — it only picks up records where `Status = New` AND `Reply` is empty
- The `Brand` table currently supports one brand/persona — to support multiple clients, add a `Profile` field to Brand and filter by matching profile name
