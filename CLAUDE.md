# Paper Notes — Agent Guide

This repo (`andreas-he/paper-notes`) is the canonical publishing home for the
AI safety journey: paper reading notes, longform posts, upskill write-ups,
research project plans, and experiment artifacts — published as a
[Quartz](https://quartz.jzhao.xyz/) site and also editable in Obsidian.

**It is PUBLIC.** Treat every commit as if it will be read by hiring managers,
reviewers, and strangers on the internet. No private life context ever lands here.

This file is the contract between the repo and any agent that edits it —
whether that's the brain (via `capture-paper` / `plan-research` skills) or
Cursor/IDE agents working locally. Read it before making changes.

## Directory Layout

```
paper-notes/
├── CLAUDE.md                          # This file
├── README.md                          # Human-facing overview
├── index.md                           # Quartz blog home
├── .gitignore
│
├── _templates/                        # Scaffolding for new notes
│   ├── paper-note.md
│   ├── topic-note.md
│   ├── research-project.md
│   ├── post.md                        # Longform post / essay / reflection
│   └── upskill-week.md                # Weekly upskill write-up
│
├── <paper-slug>.md                    # Individual paper notes (flat at root)
│
├── posts/                             # Longform posts, essays, reflections
│   └── YYYY-MM-DD-<slug>.md
│
├── upskill/                           # Weekly write-ups from the 9-month plan
│   └── week-NN-<topic>.md
│
├── research/                          # Research project plans + code plans
│   ├── <project-slug>.md
│   └── <project-slug>-code-plan.md
│
├── topics/                            # Cross-cutting topic threads
│   ├── index.md
│   └── <topic-slug>.md
│
├── code/                              # Code artifacts, per paper or project
│   └── <slug>/
│       ├── README.md                  # What lives here + how to run it
│       ├── *.py / *.ipynb
│       └── notebooks/
│
└── assets/                            # Figures, diagrams, exported plots
    └── <slug>/
```

### Why this shape
- **Paper notes stay flat at root** — Quartz resolves wikilinks by filename,
  not path. Moving them into a subdirectory would break every existing
  `[[slug]]` reference. The flat structure is load-bearing, not lazy.
- **`research/` is separated** — research project plans are fundamentally
  different from paper notes (they span multiple papers, have a timeline,
  and include implementation plans). Keeping them in their own directory
  makes the root scannable.
- **`code/<slug>/` mirrors slugs** — when a paper or research project has
  code artifacts, they live under `code/<slug>/`. The slug matches the
  paper or research project filename so the relationship is obvious.
- **`assets/<slug>/` for figures** — same slug convention for exported
  plots and diagrams.

## Content Types & Frontmatter

Every note uses YAML frontmatter. The schema depends on the content type.

### Paper note (`<slug>.md` at root)

```yaml
---
title: "Paper Title"
authors: ["Last, First", "Last, First"]
year: 2025
arxiv: "2501.12345"        # arxiv id, no URL
url: "https://..."          # canonical link (arxiv, conference, etc.)
tags: [mechanistic-interpretability, inverse-scaling]
status: reading             # reading | discussed | reviewed | implemented
date_read: 2026-04-07
date_discussed: 2026-04-08  # optional
connections:                # wikilinks to related notes
  - "[[other-paper-slug]]"
  - "[[project-slug]]"
code: "code/<slug>/"        # optional — path if we implemented something
---
```

Template: `_templates/paper-note.md`

### Research project (`research/<slug>.md`)

```yaml
---
title: "Project Title"
tags: [research-project, <topic>]
status: planning            # planning | active | writing | submitted | published
papers:                     # wikilinks to papers this project draws on
  - "[[paper-slug]]"
last_updated: 2026-04-09
---
```

Template: `_templates/research-project.md`

### Topic thread (`topics/<slug>.md`)

```yaml
---
title: "Topic Title"
tags: [topic, <area>]
papers: []
last_updated: 2026-04-09
---
```

Template: `_templates/topic-note.md`

### Longform post (`posts/YYYY-MM-DD-<slug>.md`)

```yaml
---
title: "Post Title"
date: 2026-04-23
draft: true                  # Quartz excludes from build when true
tags: [ai-safety, ...]
type: post                   # post | essay | reflection
description: "one-line summary for listings/meta"
connections:                 # wikilinks to related papers, posts, topics
  - "[[paper-slug]]"
crossposted: []              # filled after crossposting to LW / AAF
---
```

Template: `_templates/post.md`

### Upskill write-up (`upskill/week-NN-<topic>.md`)

```yaml
---
title: "Upskill Week N — Topic"
date: 2026-04-23
draft: true
tags: [upskill]
type: upskill
week: 1                      # integer, aligns with the 9-month plan
phase: "Foundation"          # Foundation | Research output | Applications
deliverable: "what the week was supposed to produce"
connections: []
---
```

Template: `_templates/upskill-week.md`

## Publish / Draft Workflow

Posts, upskill entries, and other longform items use Quartz's native `draft`
frontmatter. No custom staging pipeline:

- `draft: true` — excluded from the Quartz build
- `draft: false` — published

Publishing is `draft: true` → `draft: false` + commit. That's the whole
workflow. Paper notes and research project plans do NOT use `draft:` — they're
research artifacts, always visible.

Longform content (posts, upskill write-ups, essays) is what gets crossposted to
LessWrong / AI Alignment Forum. Canonical URL stays on this site; crossposts
carry `canonicalSource` back. Track crossposts in the `crossposted:` frontmatter
list so the post's own file is the record:

```yaml
crossposted:
  - platform: lesswrong
    url: https://lesswrong.com/posts/...
    date: 2026-04-24
```

## Status Workflow

Paper notes move through:

1. **reading** — initial capture, skimming, or partial read
2. **discussed** — read and talked through; notes captured
3. **reviewed** — fully read, notes polished, connections wired
4. **implemented** — we've built something based on it (code in `code/<slug>/`)

Research projects move through:

1. **planning** — outline and motivation drafted
2. **active** — experiments running
3. **writing** — draft in progress
4. **submitted** — under review
5. **published** — out in the world

## Naming Conventions

- **Slugs are kebab-case** — `thought-anchors`, not `ThoughtAnchors` or `thought_anchors`
- **Slugs are short** — 2–5 words. Paper title → slug is a judgment call.
- **Slugs are unique across the repo** — Quartz resolves wikilinks by filename
  only, so a collision between `papers/foo.md` and `research/foo.md` would
  break resolution. Pick distinct slugs.
- **Research project slugs** should be descriptive enough to stand alone:
  `inverse-scaling-incoherence`, not `incoherence`.

## Wikilinks

Use wikilink format `[[slug]]` — **slug only, not full path**.

```markdown
See [[thought-anchors]] for the attention-based approach.
Full plan in [[inverse-scaling-incoherence]].
```

Quartz resolves these by scanning all `.md` files and matching on filename.
Obsidian does the same when "Use unique filenames" is enabled. **Do not use
full paths in wikilinks** — `[[research/inverse-scaling-incoherence]]` will
break if the file moves.

Every cross-reference should be bidirectional: if paper A mentions research
project B, add `[[A]]` to B's `papers:` list and add `[[B]]` to A's
`connections:` list.

## Privacy Rules (MANDATORY)

This repo is public. Before committing, scan for and remove:

- **Employer names** and team/project references
- **Job application details** — fellowships, interviews, offers, rejections
- **Therapy, health, or relationship context** — anything personal
- **Financial information** — portfolio, salary, grants
- **Any other personally identifying details**

The voice should be "thoughtful researcher analyzing the field," not
"personal diary." If a paper note mentions "I discussed this with X at
my day job," rewrite it to remove the day job.

When in doubt, strip it out. Privacy violations are the hardest mistake to
reverse on a public repo — git history preserves them even after deletion.

## Code Artifacts

When a paper or research project needs code:

- Create `code/<slug>/` where `<slug>` matches the paper or research
  project filename (minus `.md`).
- Every `code/<slug>/` directory should have a `README.md` explaining
  what it does and how to run it.
- Reference it from the paper or project note via the `code:` frontmatter
  field and/or a "Code & Experiments" section with a relative link.
- Notebooks go in `code/<slug>/notebooks/` — keep them clean (clear
  outputs before committing if they contain unreviewed content).

## Commit Conventions

- Descriptive messages: `Add paper note: Thought Anchors (Bogdan et al., 2025)`
- One logical change per commit when possible
- Never commit secrets, `.env` files, or API keys
- Never commit content that references private life context (see Privacy Rules)

## Agent Workflow

When the brain's skills (`capture-paper`, `plan-research`) operate on this
repo, they follow a fixed sequence:

1. Check Notion Reading/Watch List for existing metadata
2. Derive a kebab-case slug from the paper title
3. Check for duplicates via glob
4. Write the note from the appropriate template
5. Privacy scan (the non-negotiable gate)
6. Wire bidirectional wikilinks to related notes
7. Commit and push

Cursor / IDE agents editing this repo directly should follow the same
principles: templates, slug rules, privacy scan, wikilink conventions,
and status workflow.

## What Not to Do

- Don't put paper notes in subdirectories — they live flat at root
- Don't use full paths in wikilinks — `[[slug]]` only
- Don't commit unreviewed private context — the privacy scan is mandatory
- Don't rename paper slugs after they're referenced — every wikilink
  pointing at the old slug will break
- Don't fabricate author lists, arxiv IDs, or metadata — leave fields
  empty or marked `<!-- TODO -->` rather than guessing
- Don't duplicate content between paper notes and topic threads — paper
  notes hold the analysis; topic threads link and synthesize
