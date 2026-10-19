# Migrate a local-markdown tracker to the frontmatter format

Run this when exploration found a local-markdown tracker in the older shape. Any one of these is the signal:

- `docs/agents/issue-tracker.md` is the local-markdown variant but has no `## Frontmatter` section
- a `.scratch/*/issues/*.md` file has no frontmatter but carries a `Status:`, `Blocked by:` or `Type:` line in its body (bare or bold, e.g. `**Status:**`)
- an issue file has frontmatter but no `parent` property, and its feature folder holds a `spec.md` or `map.md`

The target is the format in [issue-tracker-local.md](./issue-tracker-local.md). Read it first.

## 1. Plan

Walk every `.scratch/<feature>/` folder and work out the rewrite for each file:

- **Issue files** (`<feature>/issues/NN-*.md`): build the frontmatter from the body lines, then delete those lines.
  - `Status: x` → `status: x`
  - `Type: x` → `type: x`
  - `Blocked by: 01, 03` (numbers, or numbers with titles) → `blocked_by`, one quoted wikilink per number to the matching `NN-*.md` in the same `issues/` folder, e.g. `"[[<feature>/issues/03-slug]]"`. "None (can start immediately)" and an empty value become `[]`.
  - `parent` → `"[[<feature>/spec]]"` when `spec.md` exists, else `"[[<feature>/map]]"` when `map.md` exists, else omitted.
  - A file that already has frontmatter keeps every property it has; add only what is missing.
- **Spec files** (`<feature>/spec.md`): a `Status:` line becomes the `status` property. No other properties.
- **Maps** (`<feature>/map.md`): leave untouched.
- **Tracker doc**: rewrite `docs/agents/issue-tracker.md` from the template, carrying over any repo-specific additions the user made to the old one.
- **Base**: write [overview.base](./overview.base) to `.scratch/overview.base` when no file exists there.

Anything ambiguous stays as it is and goes on a **skipped** list with the reason: a blocker number with no matching file, two files sharing a number, a status line that names no role or wayfinding state. Never guess a link.

## 2. Confirm

Show the user the plan: how many files change per feature, one sample file before and after, and the skipped list. Edit only once the user confirms.

## 3. Rewrite

Apply the plan. Leave the changes uncommitted: `.scratch/` may live in its own repo, and committing there is the user's call. Report the files changed and repeat the skipped list so the user can fix those by hand.
