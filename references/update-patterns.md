# Update Patterns — Common Changes & Sweep Checklist

This file documents recurring update patterns observed across frndOS help center maintenance.
When a product change occurs, use the relevant pattern to systematically sweep all affected articles.

---

## Pattern 1: Navigation Change

**Trigger:** UI layout, sidebar, or navigation structure changes.

**Sweep scope:** ALL docs articles (not blog posts).

### Checklist
1. `grep -rn` for old navigation terms across all `.md` files (exclude `blog/`)
2. Replace with new terms per style-guide navigation table
3. Check "Navigate to X → Y → Z" instructions — are the paths still valid?
4. Check "Where to Find [Feature]" sections
5. Check "Before You Start" prerequisite steps that mention navigation
6. Check diagrams or workflow descriptions that reference UI structure

### History
| Date | Change | Old Term | New Term | Articles Affected |
|------|--------|----------|----------|-------------------|
| 2026-06-29 | Brand switcher moved to sidebar | "top-left dropdown / brand selector" | "brand switcher in the sidebar" | 10 docs articles |
| 2026-06-29 | Top nav replaced by left sidebar | "top navigation / header" | "left sidebar" | 4 docs articles |
| 2026-06-29 | AskFrnd gains full-screen mode | "right-side panel only" | "two modes: panel + full-screen" | 2 articles |

---

## Pattern 2: Feature Availability Change

**Trigger:** A feature previously marked as "in development", "not yet available", or "coming soon"
is now live. OR a feature previously marked as "contact support" is now self-serve.

**Sweep scope:** All articles in the affected feature's category + FAQ sections everywhere.

### Checklist
1. `grep -rn` for "in development", "coming soon", "not yet", "planned", "roadmap"
2. `grep -rn` for "contact support", "contact your account representative", "reach out to"
3. For each hit: is this still true? If the feature is now live, update the claim.
4. Check FAQ answers specifically — they're the most likely to contain stale availability claims.
5. Update any workaround instructions that are no longer needed.

### Common phrases to search for
```bash
grep -rni "in development\|coming soon\|not yet\|planned\|on the roadmap\|being built\|being worked on\|future\|upcoming" . --include="*.md" | grep -v blog/
grep -rni "contact support\|contact your.*representative\|reach out to\|provisioned by" . --include="*.md" | grep -v blog/
```

### History
| Date | Change | Old Claim | New Reality | Articles Affected |
|------|--------|-----------|-------------|-------------------|
| 2026-06-29 | Multi-account connections | "Multi-account support is in development" | Live as Extended connections | `insights/overview.md`, `connecting-paid-media.md` |
| 2026-06-29 | Brand creation self-serve | "Contact account rep to add brand" | Self-serve via sidebar `+` button | `brand-setup/multiple-brands.md` |

---

## Pattern 3: Connection / Integration Change

**Trigger:** New platform supported, connection flow changes, or connection model changes
(e.g., Core/Extended).

**Sweep scope:** All `insights/` articles + `brand-setup/multiple-brands.md`.

### Checklist
1. Check "Connecting Paid Media" — is the platform list still accurate?
2. Check "Connecting Owned Media" — any new platforms?
3. Check "Insights Overview" FAQ — connection-related answers still correct?
4. Check "Managing Multiple Brands" — does it reference connection concepts correctly?
5. If a new connection type was introduced, does it need its own article?
6. Update "Getting Started Checklist" in Insights Overview if steps changed.

---

## Pattern 4: Pillar / Tool Addition

**Trigger:** A new pillar or Studio tool launches.

**Sweep scope:** `getting-started/welcome.md`, `studio/overview.md`, pillar-specific overview.

### Checklist
1. Update Welcome article — does the "What You Can Do" section list the new pillar/tool?
2. Update Studio Overview — is the tool in the table?
3. Update Navigation article — does the sidebar description include it?
4. Create the new tool's article if warranted.
5. Cross-link from existing related articles.
6. Update registry.

---

## Pattern 5: Role / Permission Change

**Trigger:** Workspace roles renamed, added, or permission scope changed.

**Sweep scope:** `workspace/` articles + any article mentioning roles.

### Checklist
1. `grep -rn` for old role names across all docs
2. Check "Inviting Team Members" role table
3. Check "Approving Join Requests" role guidance
4. Check FAQ answers about "who can do X" across all articles

---

## Pattern 6: Blog / Announcement Divergence

**Trigger:** An announcement's content is now significantly outdated due to product evolution.

**Action:** Do NOT edit the blog post. Instead:
1. Note the divergence in the changelog
2. Consider adding an editor's note at the top of the blog post (only if explicitly requested by user)
3. Ensure the corresponding docs article has the current information

---

## General Sweep Commands

Useful grep patterns for auditing articles:

```bash
# Find all "top-left" references (should be zero in docs)
grep -rn "top-left" . --include="*.md" | grep -v blog/

# Find stale availability claims
grep -rni "in development\|coming soon\|not yet available\|planned\|roadmap" . --include="*.md" | grep -v blog/

# Find "contact support" answers (candidates for self-serve update)
grep -rni "contact.*representative\|contact.*support\|reach out\|provisioned by" . --include="*.md" | grep -v blog/

# Find all FAQ sections for manual review
grep -rn "^\\*\\*Q:" . --include="*.md" | grep -v blog/

# Find cross-links to verify integrity
grep -rn "](../" . --include="*.md"

# Find articles without Related Articles section
for f in $(find . -name "*.md" -not -path "./blog/*"); do
  grep -L "Related Articles" "$f"
done
```
