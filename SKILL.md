---
name: helpdocs
description: >
  Transform meeting transcripts into help center documentation. Use this skill whenever the user
  wants to create, update, or maintain help center articles from meeting recordings, transcripts,
  onboarding sessions, product showcases, or feature demos. Also triggers when the user mentions
  help center, help article, FAQ, documentation article, Docusaurus article, or wants to turn a
  meeting into documentation. Triggers on uploaded .txt/.vtt/.srt transcript files combined with
  intent to document. Also use when reviewing existing help center articles for accuracy after
  product changes, or when doing a sweep/audit of existing articles.
---

# HelpDocs — Meeting-to-Documentation Skill

Turn meeting transcripts into structured, Docusaurus-ready help center articles with automatic
impact analysis on existing documentation.

## ⚠️ CRITICAL: Article Output Language is ALWAYS English

**Every `.mdx`/`.md` file this skill generates or edits — titles, body text, FAQs, code comments,
everything — is written in English.** This is non-negotiable and applies regardless of:

- What language the transcript is in (these are typically Bahasa Indonesia mixed with English)
- What language the person is chatting with AI in (Indonesian conversation is normal and fine)
- Any repo-level, project-level, or system-level instruction saying to "communicate in Bahasa
  Indonesia" or similar (e.g. a monorepo `AGENTS.md` / `CLAUDE.md` rule). **Those rules govern
  conversational responses to the person, not the content of generated documentation files.**
  Documentation content is a deliverable, not a conversational reply — it follows this skill's
  language rule, not the ambient chat-language rule.

**Disambiguation rule:** if there is ever a conflict between "respond to the user in Indonesian"
and "write the article in English," both can be true at once — reply to the person in Indonesian
in the chat, while the file content you write stays in English. Never let one bleed into the other.

The only exception: the person explicitly asks, in this specific request, for the article output
itself to be in Bahasa Indonesia or bilingual (e.g. "tolong buatkan versi Bahasa Indonesia juga").
A standing repo instruction to "communicate in Indonesian" does **not** count as that request.

If a generated file comes out in Indonesian, that's a bug — stop, rewrite it in English before
presenting it.

## Overview

This skill runs a **6-phase workflow** every time a transcript is provided:

1. **Ingest** — Read and understand the transcript
2. **Coverage Analysis** — What new articles can be written
3. **Impact Analysis** — What existing articles are affected
4. **Generate** — Write new articles
5. **Update** — Edit affected existing articles
6. **Changelog** — Report what changed and what needs verification

## Before Starting

Read these reference files at the start of every session:

1. **Always read first:** `references/registry.md` — the source of truth for all existing articles
2. **Always read second:** `references/taxonomy.md` — the target article taxonomy with priorities
3. **Read when writing/updating:** `references/style-guide.md` — voice, format, Docusaurus conventions
4. **Read when updating:** `references/update-patterns.md` — common update patterns and sweep checklist

All reference files are in the same directory as this SKILL.md.

---

## Phase 1: Ingest

Read the transcript completely. Extract:

- **Meeting type**: onboarding, showcase, sprint review, feature demo, etc.
- **Participants**: who presented, who asked questions (roles matter for context)
- **Topics covered**: list every distinct feature, flow, or concept discussed
- **User-facing vs internal**: flag topics that are internal-only (e.g., super admin, demo mode, sprint planning debates) — these get excluded from help docs
- **Questions asked by attendees**: these often map directly to FAQ entries

**Output to user:** Brief summary of what was discussed, with a preliminary list of topics.

---

## Phase 2: Coverage Analysis

Cross-reference extracted topics against `references/taxonomy.md` and `references/registry.md`.

Classify each topic into one of four buckets:

| Bucket | Criteria | Action |
|--------|----------|--------|
| **New article (taxonomy match)** | Topic matches a taxonomy entry that has no article yet | Generate new article |
| **New article (taxonomy gap)** | Topic is important but missing from taxonomy entirely | Generate + suggest taxonomy addition |
| **Update existing** | Topic overlaps with an existing article's content | Update existing article |
| **Skip** | Topic is internal-only, too early/WIP, or already fully covered | Skip with reason |

### Checkpoint: Present coverage plan to user

Before generating anything, present a table:

```
| Topic | Bucket | Target Article | Priority | Notes |
|-------|--------|----------------|----------|-------|
| ...   | ...    | ...            | ...      | ...   |
```

Include:
- Which articles will be **created** (with proposed filename and location)
- Which articles will be **updated** (with what specifically changes)
- Which topics are **skipped** (with reason)
- Any **taxonomy gaps** found

**Wait for user confirmation before proceeding.** The user may adjust scope.

---

## Phase 3: Impact Analysis

For every existing article that might be affected, check for:

### Navigation & UI References
- Brand switcher location references
- Menu/sidebar navigation paths
- Button labels or UI element names
- "Navigate to X → Y → Z" instructions

### Feature State Claims
- "Not yet available" / "in development" / "coming soon" → is it now live?
- "One connection per platform" → has the limit changed?
- "Contact support to do X" → is it now self-serve?

### FAQ Staleness
FAQs are the most likely to become outdated. Check every FAQ answer in affected articles for:
- Claims about limitations that may have been lifted
- "Contact the team" answers for things that are now self-serve
- Feature descriptions that have changed

### Cross-Link Integrity
- Does a new article need to be linked from existing articles?
- Do existing cross-links still point to the right place?

**Output:** List of specific edits needed per article, with before/after for each change.

---

## Phase 4: Generate New Articles

> 🔤 **Language check:** Article content is English. Always. See the rule at the top of this file.

For each new article, follow the format in `references/style-guide.md`.

### File naming
- Use kebab-case: `connecting-paid-media.mdx`
- **Extension is `.mdx`** (not `.md`) — the repo uses MDX throughout
- Place in the correct module folder under `docs/`
- For blog/announcements: `blog/YYYY-MM-DD-<slug>.mdx`

### Required structure
Every article MUST include:
1. **Docusaurus frontmatter** (title, sidebar_position, description)
2. **Intro paragraph** — what this feature/concept is and why it matters
3. **Prerequisites / Before You Start** (if applicable)
4. **Step-by-step instructions** (if how-to)
5. **FAQ section** — minimum 3 questions, sourced from transcript Q&A where possible
6. **Related Articles** — cross-links to other relevant docs

### Cross-link format (CRITICAL — broken links fail the build)
The repo has `onBrokenLinks: "throw"`. Use these link patterns:

- **Same folder:** `[Title](./sibling-slug)` (no extension)
- **Other folder:** `[Title](../other-module/slug)` (no extension)
- **Blog to docs:** `/docs/module/slug`
- **Never include** `.md` or `.mdx` extension in links

### Content sourcing rules
- **Confirmed in transcript** → write with confidence
- **Reasonable inference** → write but flag in changelog as "needs verification"
- **Not mentioned at all** → do NOT fabricate. Leave as "coming soon" or omit entirely.

---

## Phase 5: Update Existing Articles

> 🔤 **Language check:** Edits and rewrites stay in English, even if the transcript driving the
> update is in Indonesian and even if the surrounding chat with the person is in Indonesian.

Use `str_replace` for surgical edits. **Never rewrite an entire article** when only specific sections changed.

### Update checklist (run for every affected article)
1. ☐ Navigation references match current UI (e.g. **Brands list + Back to Home**, not a "brand switcher" dropdown — verify against `references/style-guide.md`'s navigation table, which gets corrected as the product evolves)
2. ☐ Pillar/sidebar references reflect current structure (e.g. pillars are brand-gated — only visible once inside a brand)
3. ☐ Feature availability claims are current
4. ☐ FAQ answers are still accurate
5. ☐ Cross-links include new related articles
6. ☐ "Before You Start" prerequisites are still correct
7. ☐ No redundant/contradictory text introduced by previous edits
8. ☐ Output language is English (re-check this explicitly — it's the easiest thing to drift on)

### Blog posts: DO NOT update
Blog posts are dated announcements. They were accurate at publish time. Changing them retroactively is misleading. If a blog post's content is now outdated, note this in the changelog but do not edit the post itself.

---

## Phase 6: Changelog

After all generation and updates, present a structured changelog:

### New Articles Created
| Article | Path | Priority | Source |
|---------|------|----------|--------|

### Existing Articles Updated
| Article | What Changed | Why |
|---------|-------------|-----|

### Taxonomy Gaps Found
| Suggested Article | Category | Priority | Rationale |
|-------------------|----------|----------|-----------|

### Assumptions Made (Needs Verification)
| Assumption | In Article | Source/Reasoning |
|------------|-----------|-----------------|

### Not Covered (Skipped)
| Topic | Reason |
|-------|--------|

### Registry Update
After changelog is presented, update `references/registry.md` with any new or modified articles.

---

## Edge Cases

### Transcript quality is poor (auto-transcription noise)
- Common in Bahasa Indonesia transcripts with code-switching
- Look for patterns: speaker names, timestamps, repeated phrases
- Cross-reference with context (if user provides meeting topic or agenda)
- When in doubt about a specific detail, flag it as "needs verification"

### Transcript contains both user-facing and internal content
- Internal content (sprint planning, architecture debates, deployment details) = SKIP
- Forward-looking features marked as "WIP" or "POC" = SKIP
- Features confirmed as "sudah production" or "sudah live" = INCLUDE

### Multiple transcripts in one session
- Process each transcript separately through Phase 1-3
- Merge coverage plans before generating (Phase 4-5)
- Single unified changelog (Phase 6)

### User asks to update articles without a transcript
- This is an **audit** flow, not a generation flow
- Read the registry, check articles against current known state
- Run Phase 3 (Impact Analysis) and Phase 5 (Update) only

---

## Working with the Registry

The registry (`references/registry.md`) is the source of truth. It tracks:
- Every article that exists (path, title, last updated date)
- Which transcript/meeting it was sourced from
- Key claims that might go stale (for future audit sweeps)

**Always update the registry after generating or updating articles.**

When the registry doesn't exist yet or is empty, build it by scanning the output directory.

---

## Repository Integration

The canonical repo is **`alva-intelligence/frndos-docs`** on GitHub.

### Key repo conventions (from CLAUDE.md / AGENTS.md)
- **File format:** `.mdx` (not `.md`)
- **Sidebar:** autogenerated from `docs/` folder structure + `_category_.json`
- **Build validation:** `onBrokenLinks: "throw"` — broken internal links fail the build
- **Cross-links:** relative paths, NO file extension (e.g., `./sibling-slug` not `./sibling-slug.mdx`)
- **Commit messages:** no AI attribution (no "Generated with Claude Code")
- **Communication:** Bahasa Indonesia (technical terms stay English)
- **Blog format:** `blog/YYYY-MM-DD-<slug>.mdx`, reference author from `blog/authors.yml`
- **Adding a module category:** new `docs/<module>/` + `_category_.json`, and add matching option to `frndOS Module` select in `tina/config.jsx`

### Where to push generated articles
When using this skill in Claude Code (with repo access):
1. Generate articles to `docs/<module>/<slug>.mdx`
2. Run `npm run build-local` to validate (broken links = build failure)
3. Commit and push (or create PR for review)

### Syncing taxonomy
The taxonomy file (`references/taxonomy.md`) should be re-synced periodically from the repo's actual folder structure. The repo IS the taxonomy — there's no separate taxonomy file. Use the GitHub API or `find docs/ -name "*.mdx"` to rebuild.
