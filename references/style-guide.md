# Style Guide — frndOS Help Center

## Voice & Tone

### General Principles
- **Direct and confident** — state things clearly, don't hedge unnecessarily
- **Warm but not corporate** — friendly without being casual or using slang
- **Second person** — "you" not "the user"
- **Active voice** — "Click the button" not "The button should be clicked"
- **No emojis in body text** — emojis are acceptable only in section headers of overview/welcome articles and in blog posts

### Avoid
- "Simply" / "just" / "easy" — these minimize the user's effort
- "Please note that" — just state it
- "As previously mentioned" — link instead
- Overly enthusiastic language ("Amazing!" "Awesome!")
- "Currently" when describing a stable feature — only use when something is genuinely temporary

### Use
- "Heads-up:" for important gotchas (in blockquotes)
- "Tip:" for helpful shortcuts (in blockquotes)
- "Note:" for neutral supplementary info (in blockquotes)
- Em-dashes for asides — like this — especially in blog posts
- Short sentences for instructions, longer for context

---

## Article Format

### Docusaurus Frontmatter (Required)

```yaml
---
title: Human-Readable Title
sidebar_position: N
description: One-sentence summary for SEO and sidebar hover. Under 160 characters.
---
```

### Blog Post Frontmatter

```yaml
---
title: Announcement Title
date: YYYY-MM-DD
tags: [new-feature, update, improvement]
authors: [frndos-team]
description: One-sentence summary.
related_docs:
  - /docs/category/article-slug
image: ./img/banner-image-slug.png
---
```

Blog posts use `<!-- truncate -->` after the intro to set the preview cutoff.

### File Naming
- **Docs:** `kebab-case.mdx` (e.g., `connecting-paid-media.mdx`) — always `.mdx`, not `.md`
- **Blog:** `YYYY-MM-DD-slug.mdx` (e.g., `2026-04-24-brand-insights-self-serve.mdx`)
- Blog posts need `<!-- truncate -->` after the summary for preview cutoff

### Folder Structure
```
help-center/
├── getting-started/     ← onboarding, account, navigation
├── brand-setup/         ← Brand Knowledge (DNA, Identity, Tonality), multi-brand
├── studio/              ← KV Generator, Resizer, Image Editor, future tools
├── projects/            ← Project management, briefs
├── insights/            ← Dashboards, connections, tagging, KPIs
├── workspace/           ← Team management, credits, visibility, permissions
├── askfrnd/             ← AI assistant, chat generation
├── research/            ← Surveys, A/B testing, customer intelligence (future)
└── blog/                ← Announcements, changelogs
```

---

## Article Structure Templates

### How-To / Setup Article
```
# Title
Intro paragraph: what this is, why it matters.

## Before You Start
Prerequisites list.

## Step 1 — [Action]
Instructions.

## Step 2 — [Action]
Instructions.

## FAQ
Q&A pairs (minimum 3).

## Related Articles
Cross-links.
```

### Overview Article
```
# Title — Subtitle
Intro paragraph: what this pillar/tool/area is.

## What's Inside / What You Can Do
Feature table or summary.

## Where to Find It
Navigation instructions.

## [Concept/Workflow explanation]
How the parts fit together.

## Your Next Steps / Getting Started
Actionable next steps with cross-links.
```

### Announcement / Blog Article
```
# Title
Hero statement (keep original copy if provided).

<!-- truncate -->

## [Problem/Context]
Why this matters. What was the pain point.

## What's New
Feature-by-feature breakdown with cross-links to docs.

## [Walkthrough / How to Use It]
Quick end-to-end flow.

## [Caveats / Things to Know]
Honest limitations, roadmap teasers.

## CTA + Sign-off
"Head to [feature] and try it."
— The frndOS Team

## Get Started / Start Exploring
Link roundup to relevant docs.
```

---

## Cross-Linking Rules

> **CRITICAL:** The repo has `onBrokenLinks: "throw"`. A broken internal link **fails the build**.
> Always verify cross-references exist before considering a change done.

### Link format (no extension!)
- **Same folder:** `[Title](./sibling-slug)` — no `.md` or `.mdx`
- **Other folder:** `[Title](../other-module/slug)` — relative path, no extension
- **Blog to docs:** `[Title](/docs/module/slug)` — absolute path

### Examples
```mdx
<!-- Same folder (insights/) -->
[Connecting Paid Media](./connecting-paid-media)

<!-- Cross-folder (from insights/ to brand-setup/) -->
[Managing Multiple Brands](../brand-setup/multiple-brands)

<!-- Blog to docs -->
[Insights Overview](/docs/insights/overview)
```

### Cross-link direction
- Overview articles → link DOWN to detail articles
- Detail articles → link UP to overview, and ACROSS to related articles
- Blog posts → link TO docs, never the other way

---

## Tables

Use tables for:
- Feature comparisons (when to use X vs Y)
- Field descriptions (what to include, format)
- Role descriptions
- Step-by-step parameter lists

Keep tables simple. If a table has more than 5 columns, it's too complex — use prose instead.

---

## FAQ Rules

### Minimum 3 questions per article

### Source priority for FAQ content:
1. **Questions actually asked in the transcript** — highest priority, real user confusion
2. **Edge cases surfaced during discussion** — e.g., "what if I connect the wrong account"
3. **Reasonable inferences** — common questions a new user would have

### FAQ format
```
**Q: Question in natural language?**
Answer in 1-3 sentences. Link to related article if the answer needs more depth.
```

### FAQ anti-patterns (avoid)
- ❌ Questions that just repeat the article content as a question
- ❌ "What is [exact title of this article]?" — if someone is reading this, they know
- ❌ Internal-only questions (deployment, architecture)
- ❌ Questions with "contact support" as the only answer (find a better answer or omit)

---

## Navigation References (Current as of June 2026)

When describing how to navigate frndOS, use these current conventions:

**The sidebar has two states — Home and Brand. Pillars only exist inside the Brand state.**

| Element | Current Location | Correct Phrasing |
|---------|-----------------|-------------------|
| Entering a brand | **Brands** list on Home sidebar | "click a brand under **Brands** in the sidebar" |
| Switching brands | Back to Home, then Brands list | "click **Back to Home**, then pick another brand under **Brands**" |
| Pillar access (Studio, Insights, etc.) | Brand sidebar only — **brand-gated** | "open **[Pillar Name]** from the sidebar" — but only works once a brand is open |
| AskFrnd (FAB / side panel) | Bottom-right floating button, any page | "open AskFrnd from the **FAB in the bottom-right corner**" |
| AskFrnd (full screen) | Left sidebar → Chat (Home or Brand) | "open **Chat** from the **left sidebar**" |
| Workspace switcher | Left sidebar | "the workspace switcher in the sidebar" |

**Never use:** "top-left dropdown", "top navigation bar", "header menu", **"brand switcher"**
(there is no dropdown switcher — it's a **Brands** list you click into, paired with **Back to
Home** to leave). Also never imply pillars are accessible without a brand open — they're not.

**Chat history scope** (a common source of confusion — see `askfrnd/overview.mdx` for the full
breakdown): sidebar Chat is scoped to wherever you are (Home-only or current-brand-only); the
FAB always shows history across every brand and pillar.

> **Update this section** whenever navigation changes are confirmed in a product showcase. This
> table has already been corrected once after a "brand switcher" assumption turned out to be
> wrong — treat navigation claims as perishable and re-verify against screenshots/transcripts
> rather than assuming the previous description still holds.

---

## Language

### Article language: English, always
Every generated or edited `.mdx`/`.md` file is in **English** — titles, body, FAQs, frontmatter
`description`, everything. This holds **regardless of**:
- The transcript's language (typically Bahasa Indonesia mixed with English)
- The language the person is chatting in (Indonesian conversation is fine and expected)
- Any ambient "communicate in Indonesian" instruction at the repo/project/system level

**Why this needs to be explicit:** the `frndos-docs` repo's `AGENTS.md`/`CLAUDE.md` includes a
rule like *"Communicate in Bahasa Indonesia... per monorepo `.claude/rules/language.md`"*. That
rule is about **conversational replies to the person**, not about the content of generated
documentation files. If this skill is invoked inside that repo (e.g. via Claude Code), don't let
that ambient rule leak into article content — reply to the person in whatever language they're
using, but keep every file's content in English.

**The only valid exception:** the person explicitly asks, in the current request, for the article
itself to be in Bahasa Indonesia or bilingual. A standing repo-level "communicate in Indonesian"
instruction does not count as that request — it has to be specific to the documentation output.

### Target audience
External agency users and creative teams. Mix of technical and non-technical roles.

### Transcript language
Transcripts are typically **Bahasa Indonesia mixed with English** (code-switching). The skill must parse this correctly and output clean English articles.

---

## Content Sourcing Rules

| Source Level | How to Treat | Flagging |
|-------------|-------------|----------|
| **Confirmed in transcript** (speaker explicitly stated it) | Write with confidence | No flag needed |
| **Demonstrated in transcript** (speaker showed it live) | Write with confidence | No flag needed |
| **Reasonable inference** (logical from context) | Write, but mark | Flag in changelog: "Assumption — needs verification" |
| **Industry convention** (e.g., font formats TTF/OTF) | Write with hedging ("typically", "check with") | Flag in changelog |
| **Not mentioned at all** | Do NOT include | — |
| **Explicitly marked as WIP/POC/internal** | Do NOT include | Note in "Not Covered" section |
