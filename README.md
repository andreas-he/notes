# Paper Notes & Knowledge Base

<<<<<<< HEAD
A growing collection of paper reading notes, research project plans, and code
experiments — published as a [Quartz](https://quartz.jzhao.xyz/) blog and
editable in Obsidian.

For agents working in this repo, see [`CLAUDE.md`](CLAUDE.md) — it defines the
directory layout, frontmatter schema, wikilink rules, privacy constraints, and
status workflow.
=======
A growing collection of paper reading notes, posts, and upskill write-ups on the AI safety journey — published as a [Quartz](https://quartz.jzhao.xyz/) site.
>>>>>>> 2b316f7 (Expand paper-notes into the publishing home for AI safety writing (#336 Phase 1))

## Structure

```
<<<<<<< HEAD
paper-notes/
├── CLAUDE.md              # Agent guide: conventions + contracts
├── README.md              # This file
├── index.md               # Blog home page
│
├── _templates/            # Note templates
│   ├── paper-note.md
│   ├── research-project.md
│   └── topic-note.md
│
├── <paper-slug>.md        # Individual paper notes (flat at root)
│
├── research/              # Research project plans + code plans
│   └── <project-slug>.md
│
├── topics/                # Cross-cutting topic threads
│   └── <topic-slug>.md
│
├── code/                  # Experiment notebooks & scripts
│   └── <slug>/
│
└── assets/                # Figures, diagrams, exported plots
=======
papers/
├── index.md                    # Site home
├── README.md                   # This file
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
├── research/                   # Research project plans
│   └── <project-slug>.md
│
├── topics/                     # Cross-cutting topic threads
│   └── <topic-slug>.md
│
├── code/                       # Experiment notebooks, per paper or project
│   └── <slug>/
│
└── assets/                     # Figures, diagrams, exported plots
>>>>>>> 2b316f7 (Expand paper-notes into the publishing home for AI safety writing (#336 Phase 1))
    └── <slug>/
```

## Conventions

<<<<<<< HEAD
- **File names:** `kebab-case.md` matching a short slug for the paper or project
- **Links:** Use wikilinks `[[slug]]` — slug only, never full paths
- **Tags:** Use consistent tags in frontmatter for filtering
- **Status:** `reading` → `discussed` → `reviewed` → `implemented`
- **Math:** Use `$inline$` and `$$block$$` LaTeX (Quartz + Obsidian compatible)
- **Code:** Experiments live in `code/<slug>/` with a `README.md`
=======
- **Slugs:** `kebab-case.md`, unique across the whole repo (wikilinks resolve by filename, not path)
- **Paper notes** stay at the repo root so existing `[[slug]]` wikilinks keep resolving
- **Posts** live under `posts/` and are prefixed with `YYYY-MM-DD-` to preserve chronology
- **Upskill entries** live under `upskill/` as `week-NN-<topic>.md`
- **Links:** Use wikilinks `[[slug]]` for internal connections
- **Tags:** Consistent frontmatter tags for filtering
- **Status (papers):** `reading` → `discussed` → `reviewed` → `implemented`
- **Math:** `$inline$` and `$$block$$` LaTeX (Quartz + Obsidian compatible)
>>>>>>> 2b316f7 (Expand paper-notes into the publishing home for AI safety writing (#336 Phase 1))

## Publish / draft workflow

<<<<<<< HEAD
1. **Reading sessions** — paper discussions become structured notes
2. **Notion read/watch list** — existing reading database, synced periodically
3. **Code experiments** — implementations, reproductions, explorations

## Roadmap

- [x] Content structure and templates
- [x] GitHub repo (`andreas-he/paper-notes`)
- [x] Agent metadata (`CLAUDE.md`)
- [ ] Quartz setup + GitHub Pages deployment
- [ ] Notion reading list sync
- [ ] Obsidian as local editor
- [ ] Custom domain
=======
Every post, upskill entry, and long-form item uses Quartz's native `draft` frontmatter:

```yaml
draft: true   # excluded from the build
draft: false  # published
```

Publishing = flip `draft: true` → `draft: false` and commit. No custom staging pipeline.

Paper notes do not use `draft:` — they're always visible. Drafts are for writing that is meant to reach readers (posts, upskill reflections, essays).

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
- [x] Post + upskill templates and directories — #336 Phase 1, Apr 23 2026
- [x] GitHub repo (`andreas-he/paper-notes`)
- [x] Quartz + GitHub Pages deploy workflow
- [ ] First post published (currently draft: `posts/2026-04-23-open-weight-safety-threat-model.md`)
- [ ] Brain UI "Writing" tab (list, edit, promote) — #336 Phase 3, separate issue
- [ ] LessWrong crossposting automation — #336 Phase 4, separate issue
- [ ] Notion reading list sync skill
- [ ] Custom domain — owner decision pending
- [ ] Repo rename — owner decision pending
>>>>>>> 2b316f7 (Expand paper-notes into the publishing home for AI safety writing (#336 Phase 1))
