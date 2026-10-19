# Issue tracker: Local Markdown

Issues and specs for this repo live as markdown files in `.scratch/`, laid out so `.scratch/` opens as an Obsidian vault.

## Conventions

- One feature per directory: `.scratch/<feature-slug>/`
- The spec is `.scratch/<feature-slug>/spec.md`
- Implementation issues are one file per ticket at `.scratch/<feature-slug>/issues/<NN>-<slug>.md`, numbered from `01`, never a single combined tickets file
- Metadata lives in YAML frontmatter, never in body lines, so that the Obsidian base `.scratch/overview.base` can read it (see **Frontmatter** below)
- Triage state is the `status` property of each issue and spec file (see `triage-labels.md` for the role strings)
- Comments and conversation history append to the bottom of the file under a `## Comments` heading
- Open `.scratch/` itself as the Obsidian vault: every wikilink path below starts there

## Frontmatter

Issue files carry these properties; spec files carry only `status`, and maps none:

```yaml
---
status: open
type: grilling
parent: "[[<feature-slug>/spec]]"
blocked_by:
  - "[[<feature-slug>/issues/04-<issue-name>]]"
---
```

- `status`: a triage label (`triage-labels.md`) or a wayfinding state (`open`/`claimed`/`resolved`). A ticket is closed once it is `resolved` or `wontfix`.
- `type`: wayfinding tickets only (see below).
- `parent`: a quoted wikilink to the file the ticket was cut from: the feature's `spec` for tickets from a spec, the effort's `map` for wayfinding tickets. Omit it when there is neither.
- `blocked_by`: a list of quoted wikilinks to the blocking issue files, or `[]` if none.

Wikilinks are paths from `.scratch/` without the `.md` extension, and they belong in frontmatter only. Links in a file's body are relative markdown links (`[title](issues/03-slug.md)`), which read the same in Obsidian and in a plain git view.

The title stays in the body's `# NN: Title` heading.

## When a skill says "publish to the issue tracker"

Create a new file under `.scratch/<feature-slug>/` (creating the directory if needed).

## When a skill says "fetch the relevant ticket"

Read the file at the referenced path. The user will normally pass the path or the issue number directly.

## Wayfinding operations

Used by `/wayfinder`. The **map** is a file with one **child** file per ticket.

- **Map**: `.scratch/<effort>/map.md` (the Notes / Decisions-so-far / Fog body).
- **Child ticket**: `.scratch/<effort>/issues/NN-<slug>.md`, numbered from `01`, with the question in the body. Its `type` property records the ticket type (`research`/`prototype`/`grilling`/`task`); its `status` property records `open`/`claimed`/`resolved`; its `parent` property links the map (`"[[<effort>/map]]"`).
- **Blocking**: the `blocked_by` property. A ticket is unblocked when every file it lists is `resolved`.
- **Frontier**: scan `.scratch/<effort>/issues/` for files that are open, unblocked, and unclaimed; first by number wins.
- **Claim**: set `status: claimed` and save before any work.
- **Resolve**: append the answer under an `## Answer` heading, set `status: resolved`, then append a context pointer (gist + link) to the map's Decisions-so-far in `map.md`.
