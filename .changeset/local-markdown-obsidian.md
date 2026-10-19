---
"mattpocock-skills": minor
---

The local-markdown issue tracker now keeps ticket metadata in YAML frontmatter, so `.scratch/` opens as an Obsidian vault.

- Issue files carry `status`, `type` (wayfinding tickets only), `parent` and `blocked_by`. `parent` and `blocked_by` are quoted wikilinks with paths from `.scratch/`; specs carry only `status`, and maps carry no frontmatter. Body links stay relative markdown links.
- `setup-matt-pocock-skills` writes an Obsidian base to `.scratch/overview.base`, with Overview, Frontier, Claimed, Blocked and Specs & maps views.
- `to-tickets` writes local tickets with a frontmatter block (`status: ready-for-agent`, `parent` pointing at the spec, `blocked_by`) in place of the bold `Blocked by` and `Status` body lines. Wayfinding tickets gain an `open` state and a `parent` link to the map.
- Existing `.scratch/` trackers that keep `Status:` and `Blocked by:` lines in the body need converting. Re-run `/setup-matt-pocock-skills`: it detects the older shape, shows a migration plan, and rewrites the files only once you confirm, leaving the result uncommitted. Anything it cannot convert unambiguously is listed for you to fix by hand.
