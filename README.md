# helpdocs — Meeting-to-Documentation Skill for frndOS

A Claude skill that transforms meeting transcripts into structured, Docusaurus-ready help center
articles for [frndos-docs](https://github.com/alva-intelligence/frndos-docs) — with automatic
impact analysis on existing documentation.

## What This Does

Drop in a meeting transcript (onboarding session, product showcase, feature demo) and this skill:

1. **Ingests** the transcript and identifies user-facing topics
2. **Analyzes coverage** against the existing taxonomy and article registry
3. **Analyzes impact** — flags existing articles affected by navigation/feature changes
4. **Generates** new `.mdx` articles matching frndOS Help Center conventions
5. **Updates** existing articles surgically (not full rewrites)
6. **Reports** a changelog with assumptions flagged for verification

## Structure

```
helpdocs/
├── SKILL.md                    # Main workflow (6 phases)
└── references/
    ├── registry.md              # Source of truth: all existing articles + staleness watch
    ├── taxonomy.md               # Target article taxonomy, synced from frndos-docs repo
    ├── style-guide.md            # Voice, format, Docusaurus/MDX conventions
    └── update-patterns.md        # Reusable sweep patterns (nav changes, feature flags, etc.)
```

## How to Use

### Option A — Claude Code (recommended)

This skill is designed to be dropped directly into the `frndos-docs` repo so Claude Code can
read existing articles, write new `.mdx` files, validate the build, and commit changes.

```bash
# From the root of your frndos-docs checkout
git clone https://github.com/<your-username>/helpdocs-skill.git .claude/skills/helpdocs
```

Then in Claude Code, inside the `frndos-docs` repo:

```
> buatkan article dari meeting transcript ini [paste or attach]
```

Claude will read `SKILL.md`, run the 6-phase workflow, and write `.mdx` files directly into
`docs/<module>/`.

### Option B — claude.ai (Skills feature)

1. Download this repo as a zip
2. Go to **claude.ai → Settings → Capabilities → Skills**
3. Upload the `helpdocs/` folder

Output will be `.md` files in the conversation — copy them into your repo manually and rename
to `.mdx`.

## Keeping This in Sync

The taxonomy and registry are snapshots of the `frndos-docs` repo at the time of last sync.
Re-sync periodically:

```bash
# From inside frndos-docs
find docs -name "*.mdx" | sort
```

Update `references/taxonomy.md` and `references/registry.md` to match.

## License

Internal — Alva Intelligence / frnd.
