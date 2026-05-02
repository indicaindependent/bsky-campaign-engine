<div align="center">

# 📡 Bluesky Campaign Engine

**Automated Bluesky campaign scheduling — AT Protocol + Cloudflare D1 + thread chaining**

[![Bluesky](https://img.shields.io/badge/AT_Protocol-0085ff?style=for-the-badge&logo=bluesky&logoColor=white)](https://atproto.com)
[![Cloudflare Workers](https://img.shields.io/badge/Cloudflare_Workers-F38020?style=for-the-badge&logo=cloudflare&logoColor=white)](https://workers.cloudflare.com)
[![License: MIT](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)

</div>

---

## What Is This

A production-grade campaign scheduling engine for Bluesky. Schedule multi-post thread campaigns with images, facets (links/mentions/tags), and precise timing — all running serverless on Cloudflare's edge.

Built and battle-tested running real investigative journalism campaigns on Bluesky.

---

## Features

- 🧵 **Thread Chaining** — proper reply chains with root/parent URI tracking
- 🖼️ **Image Embedding** — automatic image upload + blob attachment
- 🏷️ **Facet Support** — links, mentions, hashtags auto-detected and encoded
- ⏱️ **D1-Based Scheduling** — fire at precise times via CF Cron
- 🔒 **Fire-Once Lock** — D1 state prevents double-posting
- 📦 **Campaign Archive** — completed campaigns archived, never deleted
- 🌐 **REST API** — inject campaigns via `/inject`, check status via `/status`

---

## Architecture

```
POST /inject (campaign payload)
        │
        ▼
schedule-worker (D1: schedule-db)
        │
CF Cron Trigger (every minute check)
        │
        ▼
bsky-worker ──► AT Protocol API ──► Bluesky
        │
        └── Update D1 (post URIs, CIDs, status)
```

---

## Campaign Payload Format

```json
{
  "campaign_id": "my-campaign-001",
  "handle": "yourhandle.bsky.social",
  "posts": [
    {
      "post_index": 0,
      "text": "🧵 THREAD: My investigation into X...",
      "image_url": "https://example.com/image.jpg",
      "scheduled_at": "2026-05-02T14:00:00Z"
    },
    {
      "post_index": 1,
      "text": "1/ Here's what we found...",
      "scheduled_at": "2026-05-02T14:05:00Z"
    }
  ]
}
```

---

## Self-Hosting

```bash
git clone https://github.com/indicaindependent/bsky-campaign-engine
cd bsky-campaign-engine

cp wrangler.toml.example wrangler.toml

wrangler d1 create campaign-db
wrangler secret put BSKY_APP_PASS
wrangler secret put WORKER_SECRET

wrangler deploy
```

---

## Grapheme Safety

All posts are validated against Bluesky's 300-character limit (JS `length`, not byte count). Flag emoji = 4 chars. Hard ceiling enforced before posting.

---

## License

[MIT](LICENSE)

---

<div align="center">
<sub>Battle-tested on real investigative campaigns | Built by <a href="https://osintnet.uk">Indica Independent</a></sub>
</div>
