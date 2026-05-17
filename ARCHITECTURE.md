# Clipwise — Technical Architecture

## System Overview

Clipwise is a knowledge-driven AI platform for short-form video optimization. Unlike wrapper tools that send raw prompts to LLMs, Clipwise uses a multi-layer architecture where every AI call is grounded in curated domain knowledge, platform-specific benchmarks, and psychological frameworks.

```
User Request
     │
     ▼
┌─────────────────────────────────────────────┐
│           FEATURE ROUTER                     │
│  (API route determines task type)            │
│  adapt | analyze | script | audit | marketer │
└──────────┬──────────────────────────────────┘
           │
           ▼
┌─────────────────────────────────────────────┐
│        KNOWLEDGE SELECTOR                    │
│  Loads only the files relevant to this task  │
│                                              │
│  Script generation → 6 files                 │
│  Trend adaptation  → 7 files                 │
│  Post-analysis     → 4 files                 │
│  Profile audit     → 5 files                 │
│  AI Marketer       → 14 files                │
│  Email generation  → 1 playbook              │
└──────────┬──────────────────────────────────┘
           │
           ▼
┌─────────────────────────────────────────────┐
│        CONTEXT BUILDER                       │
│                                              │
│  ┌──────────┐ ┌──────────┐ ┌──────────────┐ │
│  │ Knowledge │ │ User     │ │ Platform     │ │
│  │ Playbooks │ │ Profile  │ │ Benchmarks   │ │
│  └────┬─────┘ └────┬─────┘ └──────┬───────┘ │
│       │             │              │          │
│       ▼             ▼              ▼          │
│  ┌──────────────────────────────────────┐    │
│  │  Merged prompt with all context      │    │
│  │  + brand voice + avatar targeting    │    │
│  │  + humanizer rules                   │    │
│  └──────────────────────────────────────┘    │
└──────────┬──────────────────────────────────┘
           │
           ▼
┌─────────────────────────────────────────────┐
│        AI PIPELINE                           │
│                                              │
│  Video input ──► Gemini 2.0 Flash            │
│                  (watches actual frames)      │
│                         │                     │
│                         ▼                     │
│  Gemini analysis ──► Claude 3.5 Sonnet       │
│                      (with knowledge context) │
│                         │                     │
│                         ▼                     │
│  Structured JSON response                     │
└──────────┬──────────────────────────────────┘
           │
           ▼
┌─────────────────────────────────────────────┐
│        OUTPUT LAYER                          │
│  Scores, scripts, action plans, benchmarks   │
│  + humanizer validation                      │
│  + platform-specific formatting              │
└─────────────────────────────────────────────┘
```

## Knowledge Base Architecture

The knowledge base is the core differentiator. It's a collection of **35+ curated playbook files** compiled from 1,300+ marketing, psychology, and video strategy sources. Files are organized by feature and loaded on-demand — not dumped into every prompt.

### Knowledge Categories

| Category | Files | Used By | What It Contains |
|----------|-------|---------|------------------|
| Hook Frameworks | 1 | Adapt, Script, Audit | 15 hook types with psychological trigger mapping, visual hook strategy, testing framework |
| Script Structures | 2 | Script Writer, Adapt | 8 proven structures (PAS, Story Arc, List, etc.) with beat-by-beat timing, retention engineering |
| Copywriting Formulas | 1 | Script, Adapt, Email | 20+ formulas (AIDA, PASTOR, BAB, CUT, PROVE) with video-specific adaptations |
| Psychology Triggers | 1 | All features | 18 cognitive biases with power ratings, combination matrix by video type, anti-patterns |
| Viral Adaptation | 3 | Trend Adaptation | Frame-by-frame replication method, structural DNA extraction, cross-culture adaptation rules |
| Platform Benchmarks | 3 | Post-Analysis, Audit | Engagement rate norms by follower tier, dropout classification, completion rate benchmarks |
| Post-Analysis | 4 | Video Analysis | Goal-specific analysis (reach/expertise/sales), estimation ratios, temporal analysis rules |
| Profile Audit | 6 | Audit feature | Platform-specific methodology (TikTok/Instagram), scoring framework, content pillar analysis |
| Script Generation | 9 | Script Writer | Format-specific playbooks (tutorial, storytelling, lifehack, listicle, reaction, review, POV, mini-vlog) |
| Humanizer Rules | 1 | Script, Adapt | AI-writing detection avoidance: banned words, sentence variety rules, platform-specific tells |
| Email Playbook | 1 | Email system | Funnel stages, copy formulas, frequency rules, personalization framework |
| AI Marketer | 14 | Marketer chat | Domain-specific knowledge (ads, analytics, psychology, UGC, brand building, etc.) |

### How Knowledge Injection Works

```
1. Request arrives (e.g., "generate a tutorial script about cooking")

2. Feature router identifies: task = script_generation, format = tutorial

3. Knowledge selector loads:
   - gen-scripts.md          (universal script rules)
   - gen-script-tutorial.md  (tutorial-specific structure)
   - humanizer-rules.md      (AI detection avoidance)
   + user's brand voice, avatar data from Supabase

4. Context builder merges all into a single prompt:
   ┌────────────────────────────────────┐
   │ System: "You are a scriptwriter"   │
   │ + SCRIPT PLAYBOOK (gen-scripts)    │
   │ + FORMAT RULES (tutorial)          │
   │ + HUMANIZER RULES                  │
   │ + BRAND VOICE: "casual, energetic" │
   │ + AVATAR: "25-35 beginner cooks"   │
   │ + TASK: specific user request      │
   └────────────────────────────────────┘

5. Claude generates structured JSON response

6. Output returned to user with scores and metadata
```

### Why This Matters

| Approach | What AI sees | Output quality |
|----------|-------------|----------------|
| **Generic wrapper** | "Write a TikTok script about cooking" | Generic, sounds like ChatGPT, no structure |
| **Prompt-engineered** | Same + a long system prompt | Better structure, but static and one-size-fits-all |
| **Clipwise (knowledge-driven)** | Task-specific playbook + format rules + platform benchmarks + psychology triggers + humanizer + user context | Structurally proven, psychologically grounded, sounds human, personalized |

## Dropout Diagnostics (Example of Knowledge Depth)

Post-publish analysis doesn't just say "your video needs improvement." It diagnoses the exact failure point using retention curve classification:

| Dropout Zone | Diagnosis | Prescribed Fix |
|-------------|-----------|----------------|
| 0-10% of video | Hook failure | Rebuild first 3 seconds with pattern interrupt, state value proposition immediately |
| 10-30% | Pacing failure | Add subhook at 5-8s mark, increase cut frequency, add text overlay with new promise |
| 30-60% | Relevance drop | Content drifted from hook promise — identify exact timestamp, restructure middle |
| 60%+ | Length issue | Video longer than topic warrants — cut final third, add CTA teaser at 60% mark |

Each zone has specific root causes, symptoms, and a multi-step fix protocol. This is loaded from the knowledge base, not hardcoded in prompts.

## Email Intelligence System

Lifecycle emails are AI-generated with per-user personalization:

```
┌──────────────────────────────┐
│     USER ACTIVITY DATA        │
│  signup date, last activity,  │
│  total actions, niche, goals  │
└──────────┬───────────────────┘
           │
           ▼
┌──────────────────────────────┐
│     FUNNEL STAGE DETECTOR     │
│  cold → warm → active →      │
│  at_risk → churned            │
└──────────┬───────────────────┘
           │
           ▼
┌──────────────────────────────┐
│     FORMULA SELECTOR          │
│  PAS for cold, BAB for warm,  │
│  CUT for at_risk, etc.        │
└──────────┬───────────────────┘
           │
           ▼
┌──────────────────────────────┐
│     AI EMAIL GENERATION       │
│  Claude + email playbook +    │
│  user profile + formula +     │
│  psychological triggers       │
└──────────┬───────────────────┘
           │
           ▼
┌──────────────────────────────┐
│     FREQUENCY GOVERNOR        │
│  max 2/week, 3-day minimum,   │
│  pause after 3 unopened       │
└──────────┬───────────────────┘
           │
           ▼
┌──────────────────────────────┐
│     TRACKING & ANALYTICS      │
│  Open pixel, click redirect,  │
│  per-campaign metrics         │
└──────────────────────────────┘
```

## AI Marketer Memory Architecture

The AI Marketer maintains persistent context per user (or per client for agencies):

```
User conversation
       │
       ▼
┌──────────────────────────────────┐
│  CONTEXT LOADER                   │
│                                   │
│  From Supabase:                   │
│  - Marketing profile (niche,      │
│    goals, pains, brand voice)     │
│  - Customer avatars               │
│  - Conversation history           │
│  - Past analyses & metrics        │
│                                   │
│  From Knowledge Base:             │
│  - 14 domain-specific documents   │
│    (loaded by topic detection)    │
│  - psychology.md                  │
│  - hook-copywriting.md            │
│  - video-analytics.md             │
│  - paid-ads.md                    │
│  - content-strategy.md            │
│  - ... (10 more)                  │
└──────────┬───────────────────────┘
           │
           ▼
┌──────────────────────────────────┐
│  Claude generates response with:  │
│  - Domain expertise from KB       │
│  - User-specific context          │
│  - Actionable recommendations     │
│  - Memory of past conversations   │
└──────────────────────────────────┘
```

## Tech Stack

| Layer | Technology | Why |
|-------|-----------|-----|
| Framework | Next.js 14 (App Router) | SSR + API routes + i18n in one |
| Database | Supabase (PostgreSQL + RLS) | Auth + realtime + row-level security |
| AI — Vision | Gemini 2.0 Flash | Watches actual video frames (not just metadata) |
| AI — Intelligence | Claude 3.5 Sonnet | Structured reasoning with knowledge injection |
| Video Processing | FFmpeg WASM | Client-side compression, no server upload |
| Email | Resend + Claude | AI-personalized lifecycle emails with tracking |
| Payments | WayForPay | Ukrainian market (Visa/MC in UAH) |
| Rate Limiting | Upstash Redis | Per-user rate limits on all API routes |
| Deployment | Vercel | Auto-deploy, edge functions, cron |
| i18n | next-intl | Ukrainian + English |
| Social Data | Apify | TikTok, Instagram, YouTube scraping |

## Security & Access Control

- All API routes require authentication via Supabase Auth (SSR)
- Rate limiting on every public endpoint (Upstash Redis)
- Row-Level Security in PostgreSQL — users can only access their own data
- Service role key used only in server-side cron jobs
- Token-based usage tracking prevents abuse
- Knowledge files are NOT included in the public repository

## Metrics & Scale

- **35+ AI knowledge documents** powering all features
- **20+ countries** supported for trend search
- **4 platforms** analyzed (TikTok, Instagram Reels, YouTube Shorts, Facebook Reels)
- **15 hook frameworks** × **8 script structures** × **20+ copy formulas** = thousands of unique output combinations
- **18 psychological triggers** with proven combination matrix
- **5 funnel stages** with stage-specific email strategies
- **Bilingual** (Ukrainian + English) with full i18n

---

*Architecture documentation for grant applications and technical review. Core knowledge base files are proprietary and not included in this repository.*
