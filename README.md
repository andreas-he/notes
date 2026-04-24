# Paper Notes & Knowledge Base

A growing collection of paper reading notes, posts, and upskill write-ups on the AI safety journey — published as a [Quartz](https://quartz.jzhao.xyz/) site and editable in Obsidian.

For agents working in this repo, see [`CLAUDE.md`](CLAUDE.md) — it defines the directory layout, frontmatter schema, wikilink rules, privacy constraints, and status workflow.

## Structure

```
paper-notes/
├── CLAUDE.md                   # Agent guide: conventions + contracts
├── README.md                   # This file
├── index.md                    # Site home
│
├── _templates/                 # Note templates
│   ├── paper-note.md           # Per-paper discussion
│   ├── research-project.md     # Research project plan
│   ├── topic-note.md           # Cross-paper topic thread
│   ├── post.md                 # Longform post / essay / reflection
│   └── upskill-week.md         # Weekly upskill write-up
│
├── <paper-slug>.md             # Paper notes — flat at root (wikilink resolution)
│
├── posts/                      # Longform posts, essays, research reflections
│   └── YYYY-MM-DD-<slug>.md
│
├── upskill/                    # Weekly write-ups from the 9-month plan
│   └── week-NN-<topic>.md
│
├── research/                   # Research project plans + code plans
│   └── <project-slug>.md
│
├── topics/                     # Cross-cutting topic threads
│   └── <topic-slug>.md
│
├── code/                       # Experiment notebooks, per paper or project
│   └── <slug>/
│
└── assets/                     # Figures, diagrams, exported plots
    └── <slug>/
```

## Conventions

- **Slugs:** `kebab-case.md`, unique across the whole repo (wikilinks resolve by filename, not path)
- **Paper notes** stay at the repo root so existing `[[slug]]` wikilinks keep resolving
- **Posts** live under `posts/` and are prefixed with `YYYY-MM-DD-` to preserve chronology
- **Upskill entries** live under `upskill/` as `week-NN-<topic>.md`
- **Links:** Use wikilinks `[[slug]]` for internal connections — slug only, never full paths
- **Tags:** Consistent frontmatter tags for filtering
- **Status (papers):** `reading` → `discussed` → `reviewed` → `implemented`
- **Math:** `$inline$` and `$$block$$` LaTeX (Quartz + Obsidian compatible)

## Publish / draft workflow

Posts, upskill entries, and other longform material use Quartz's native `draft` frontmatter — no custom staging pipeline:

```yaml
draft: true   # excluded from the Quartz build
draft: false  # published
```

Publishing is `draft: true` → `draft: false` + commit. Paper notes do not use `draft:` — they're reading artifacts, always visible.

## Content sources

1. **Reading sessions with the brain** — paper discussions become structured notes (`capture-paper` skill)
2. **Notion read/watch list** — reading database, synced periodically
3. **Upskill weekly reviews** — synthesized from weekly-review + work logs into `upskill/`
4. **Original posts** — threat model essays, research reflections, field commentary

## Crossposting (manual for now)

Canonical home is this repo. Longform posts get crossposted to LessWrong / AI Alignment Forum with `canonicalSource` pointing back. Frontmatter tracks crossposts:

```yaml
crossposted:
  - platform: lesswrong
    url: https://lesswrong.com/posts/...
    date: 2026-04-24
```

Crossposting is currently a manual paste; API automation is a later phase.

## Roadmap

- [x] Content structure and templates (paper, research, topic)
- [x] GitHub repo (`andreas-he/paper-notes`)
- [x] Agent metadata (`CLAUDE.md`)
- [x] Post + upskill templates and directories — #336 Phase 1, Apr 23 2026
- [x] #336 Phase 1 decisions locked (2026-04-24): repo will rename to `notes`; site display name "Andreas Hermann — Notes"; domain deferred; `posts/` kept; upskill public by default; crossposts opt-in
- [ ] Repo rename executed (`paper-notes` → `notes`, owner-manual)
- [ ] Quartz setup + GitHub Pages deployment (#336 Phase 2)
- [ ] First post published (currently draft: `posts/2026-04-23-open-weight-safety-threat-model.md`)
- [ ] Brain UI "Writing" tab (list, edit, promote) — #336 Phase 3, separate issue
- [ ] LessWrong crossposting automation — #336 Phase 4, separate issue
- [ ] Notion reading list sync skill
- [ ] Obsidian as local editor
- [ ] Custom domain (deferred until ≥2–3 published posts)
