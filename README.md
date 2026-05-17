# Clipwise — Knowledge-Driven AI for Short-Form Video

> AI platform that helps creators make better videos using psychology, proven frameworks, and real platform benchmarks — not generic AI advice.

🌐 **[tryclipwise.com](https://tryclipwise.com)** · Free plan available · EN / UK

---

## The Problem

Content creators waste hours on videos that flop. Existing AI tools give generic advice ("make a better hook") without understanding WHY videos succeed at a structural and psychological level. They don't know platform benchmarks, can't diagnose specific failure points, and their output sounds obviously AI-generated.

## The Solution

Clipwise is a **knowledge-driven** AI platform. Every AI call is backed by 35+ curated playbook files — compiled from 1,300+ marketing, psychology, and video strategy sources. The system doesn't just prompt an LLM; it loads task-specific knowledge, applies proven frameworks, validates against platform benchmarks, and enforces humanization rules.

**Result:** Output that's structurally sound, psychologically effective, platform-calibrated, and sounds like a real creator — not ChatGPT.

## What Makes Clipwise Different

| Other tools | Clipwise |
|-------------|----------|
| Generic prompt → generic output | Task-specific knowledge injection (6-14 files per call) |
| "Improve your hook" | "Your dropout is at 12% = pacing failure. Add subhook at 5-8s, increase cut frequency" |
| One-size-fits-all scripts | Format-specific playbooks (tutorial vs storytelling vs review — different rules) |
| AI-sounding output | Humanizer layer: banned words, sentence variety, anti-AI-detection rules |
| No context memory | Persistent brand voice, audience avatars, conversation history per client |
| Flat pricing advice | Platform-specific benchmarks by account tier (nano/small/mid/large) |

## Core Features

### AI Video Analysis
- **Pre-publish:** Gemini 2.5 Flash watches actual video frames → scores Hook/Pace/Value/CTA/Quality (0-100) with timestamped fixes
- **Post-publish:** Dropout zone diagnostics (0-10% hook fail, 10-30% pacing, 30-60% relevance, 60%+ length), engagement benchmarking vs platform norms by account size

### Trend Adaptation
- Frame-by-frame viral format replication using 7 knowledge layers
- Structural DNA extraction → content mapping → cross-culture adaptation
- Psychology triggers preserved while content changes

### AI Script Writer
- 15 hook types × 8 script structures × 20+ copywriting formulas
- Format-specific playbooks (8 formats with unique rules)
- Humanizer validation — output passes AI-writing detection avoidance
- Beat-by-beat timing, camera direction, text overlays, audio cues

### AI Marketer with Memory
- Persistent AI consultant backed by 14 specialized knowledge documents
- Remembers niche, brand voice, audience avatars, past analyses
- Per-client profiles for agencies (separate AI memory per client)

### Smart Email System
- AI-personalized lifecycle emails using Claude + psychological copy formulas
- 5-stage funnel detection (cold → warm → active → at_risk → churned)
- Behavioral triggers: milestones, trend alerts, re-engagement
- Anti-spam: frequency capping, automatic pause after 3 unopened

### Profile Audit
- 6-dimension health score (positioning, content quality, hook mastery, engagement, strategy, growth)
- Platform-specific methodology for TikTok and Instagram
- Content pillar analysis with engagement correlation

## Architecture

See [ARCHITECTURE.md](./ARCHITECTURE.md) for the full technical breakdown including:
- Knowledge injection pipeline
- AI processing flow (Gemini → Claude with context)
- Email intelligence system
- Memory architecture for AI Marketer

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Framework | Next.js 14 (App Router, TypeScript) |
| Database & Auth | Supabase (PostgreSQL + Row-Level Security) |
| AI — Vision | Gemini 2.5 Flash |
| AI — Intelligence | Claude 4.6 Sonnet (Anthropic) |
| Knowledge Base | 35+ proprietary playbook files |
| Video Processing | FFmpeg WASM (client-side) |
| Email | Resend + AI personalization |
| Payments | WayForPay (Ukraine) |
| Deployment | Railway |
| i18n | next-intl (Ukrainian + English) |

## Pricing

| Plan | Price | Tokens/mo | Key Features |
|------|-------|-----------|--------------|
| Free | $0 | 200 | Trend search, basic video analysis |
| Pro | $24/mo | 2,000 | All features + AI Marketer + Content Plan |
| Agency | $73/mo | 7,000 | + Script Writer + multi-client + unlimited Marketer |

## Scale & Numbers

- **35+ knowledge documents** powering all AI features
- **1,300+ sources** compiled into the knowledge base
- **20+ countries** for trend research
- **4 platforms:** TikTok, Instagram Reels, YouTube Shorts, Facebook Reels
- **15 hook types × 8 structures × 20 formulas** = unique combinations per request
- **18 psychological triggers** with proven combination matrix
- **Bilingual:** Ukrainian + English (full i18n)

## MCP Integration

Clipwise supports the **Model Context Protocol** — Claude Desktop, Cursor, Windsurf, and any MCP-compatible AI can call Clipwise tools natively.

```json
{
  "mcpServers": {
    "clipwise": {
      "command": "npx",
      "args": ["-y", "clipwise-mcp-server"]
    }
  }
}
```

npm: [`clipwise-mcp-server`](https://www.npmjs.com/package/clipwise-mcp-server) · Source: [clipwise-mcp](https://github.com/mobileshop9991-star/clipwise-mcp)

## AI Discoverability

- [tryclipwise.com/llms.txt](https://tryclipwise.com/llms.txt) — service description for AI models
- [tryclipwise.com/pricing.md](https://tryclipwise.com/pricing.md) — structured pricing for AI agents
- JSON-LD schema markup on all pages
- All AI crawlers explicitly allowed in robots.txt

## Founder

**Oleksandr Petrov** (Олександр Петров) — indie maker and content creator from Vinnytsia, Ukraine.

- 🎬 TikTok: [@Lusuy_sharit](https://www.tiktok.com/@Lusuy_sharit)
- 📸 Instagram: [@Lusuy_sharit](https://www.instagram.com/Lusuy_sharit)
- 📫 Email: alexander@tryclipwise.com
- 🌐 Website: [tryclipwise.com](https://tryclipwise.com)

---

## Repository Note

This is the main Clipwise repository. The **knowledge base files (35+ proprietary playbooks)** and **AI prompt logic** are not included in the public repository as they represent core intellectual property. The [ARCHITECTURE.md](./ARCHITECTURE.md) document describes the system design and approach in detail.

---

*Built with Next.js · Supabase · Gemini AI · Claude · Apify*
