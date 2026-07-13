---
name: check-doc-structure
description: >-
  Audit documentation structure and navigation for forge docs — required
  frontmatter, heading hierarchy, section order, index/nav coverage,
  link and anchor integrity, orphan pages, and duplicate or misplaced
  topics. Use when asked to check doc structure, after adding or moving
  a doc page, or when a page becomes hard to discover from the docs nav.
---

# Check documentation structure

This skill verifies that forge documentation is **structurally sound and
navigable**: pages have the expected frontmatter, headings appear in the
right order, links resolve, anchors exist, and docs show up where readers
expect them in the site nav and index pages.

It is repeatable — run it any time a page is added, renamed, moved, or
split, and especially before publishing a docs-heavy change.

## What structure means here

Structure is the page-level and site-level scaffolding that makes docs
usable. The exact expectations depend on the doc family, but the same
classes of drift appear everywhere:

| Layer | What to check | Source of truth |
|---|---|---|
| Page metadata | frontmatter fields, title, sidebar order, quadrant/category markers | the doc family conventions in `docs/` and `architecture/` |
| Heading hierarchy | H1/H2/H3 order, required section names, no skipped levels | the current page template for that doc type |
| Navigation | VitePress sidebar, index pages, cross-links from sibling docs | `docs/.vitepress/` and the relevant section index |
| Links and anchors | relative links, fragment anchors, code-block references | the linked page and its actual heading text |
| Coverage | orphan pages, duplicate topics, stale renamed paths | the directory tree under `docs/` and `architecture/` |

A structural finding is any page that cannot be discovered, rendered,
or read in the expected order because its scaffolding drifted.

## Depth modes

This skill should be usable at different depths without becoming a new
skill for each one:

- **quick** — one page plus its direct links and parent index.
- **standard** — one doc family, such as a guide or reference section.
- **deep** — repository-wide navigation and path consistency across all
  doc roots.

Use the same criteria at each depth; only the scope changes.

## Procedure

Work through these steps in order and report findings at the end.

### 1. Identify the doc family

Decide whether the target is a guide, reference page, architecture
chapter, README, or generated artifact. The required sections and nav
expectations come from that family, not from a single universal template.

### 2. Check the page skeleton

Verify the page has the expected metadata and outline:

- frontmatter exists and is valid
- title matches the page purpose
- required headings appear in the expected order
- no empty or duplicated sections remain after edits
- code blocks and admonitions are not breaking the flow

### 3. Check discoverability

Confirm the page is reachable from the places readers actually use:

- sidebar or nav config
- section index pages
- sibling cross-links
- any landing page that lists the topic

### 4. Check links and anchors

Resolve every local link and fragment anchor that the page introduces or
updates. If a link target moved, update the source and the destination
path together.

### 5. Report

Produce a concise summary grouped by severity:

- **HIGH** — a page cannot be found through the documented nav; a link
  target is missing; the page skeleton is broken enough that the doc
  family no longer renders as intended.
- **MEDIUM** — a required section is missing or out of order; an index or
  sidebar entry is stale; a renamed page still has old inbound links.
- **LOW** — a cosmetic heading-level mismatch, duplicate wording, or a
  stale anchor that still resolves after redirects.

For each finding give one line with a file reference for both the source
page and the nav/index page when applicable. End with a per-doc-family
verdict.

## Notes

- This skill is read-only; apply fixes in a separate, explicitly scoped
  task.
- Pair this with [[check-doc-expressions]] for prose quality and with
  [[check-doc-consistency]] for cross-document truth alignment.
- If a generated doc or a site template is involved, verify the generator
  or template separately before patching the rendered page.
