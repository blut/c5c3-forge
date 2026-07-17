---
name: check-doc-expressions
description: >-
  Audit documentation prose quality for forge docs — sentence clarity,
  active voice, terminology, ambiguity, tone, jargon control, and the
  readability of command examples and code-adjacent explanations. Use
  when asked to improve writing quality, after drafting or editing prose,
  or when a page reads correctly but not clearly.
---

# Check documentation expressions

This skill verifies that forge documentation **reads cleanly and says
exactly what it means**. It focuses on prose quality rather than page
shape: sentences should be direct, terminology should be consistent,
examples should be readable, and important claims should not be buried
in vague language.

It is repeatable — run it any time text changes materially, especially in
user-facing guides, tutorials, reference prose, or release notes.

## What expressions means here

Expression quality is the writing layer that sits on top of structure.
The content can be technically correct and still be hard to use if the
prose is muddy or inconsistent.

| Area | What to check | Source of truth |
|---|---|---|
| Sentence clarity | short, direct sentences; one idea per sentence; no dangling references | the intended reader outcome |
| Voice and tone | active voice, concrete verbs, minimal hedging; no marketing or aspirational phrasing ("the shortest path to…", "seamlessly") in operational docs | the doc family style used elsewhere in the repo |
| Terminology | one term per concept, no accidental synonyms | the repo glossary and established usage |
| Command examples | commands are complete, ordered, and copyable | the real workflow the docs describe |
| Explanatory text | definitions appear before specialized terms are used | the implementation or process being documented |
| Unexplained references | named tools, daemons, helper processes, or APIs (e.g. a background service invoked indirectly, an internal API) are explained or linked on first use **on the page they appear on** — a definition living only in a different page does not count | the reader landing on this specific page, not the whole corpus |
| Prerequisite accuracy | stated tool/version requirements match what the workflow actually needs (minimum versions, why a tool is required, what it's required *for*) | the scripts/Makefile/CI steps the doc walks the reader through |
| Operational realism | the page covers what to do when a step fails, times out, or needs a retry — not only the happy path; environment assumptions (network, resources) that can make a step fail are stated as prerequisites | known failure reports and support history for that workflow |
| Depth parity | every step/section of one walkthrough gets a comparable level of explanation — no step is a bare command while its siblings get paragraphs of context | the other steps on the same page |
| Readability | long paragraphs, repetition, and filler are removed | the page's purpose and audience |

A writing finding is any place where the prose forces the reader to infer
meaning that the document should have stated directly.

## Depth modes

Use the same skill at multiple depths rather than splitting it into more
skills:

- **quick** — one page, line-level wording cleanup.
- **standard** — one document set, such as a guide and its reference
  neighbors.
- **deep** — repo-wide terminology and phrase consistency.

## Procedure

Work through these steps in order and report findings at the end.

### 1. Identify the audience

Decide whether the page is for a new contributor, an operator, a release
owner, or a reader looking up a fact. The same sentence can be clear for
one audience and opaque for another.

### 2. Read for meaning, not grammar alone

Check whether each paragraph does the following:

- states the point up front
- uses the same term for the same concept
- avoids unexplained acronyms, jargon, or named tools/daemons/APIs that
  this page never defines (checking the page itself, not whether some
  other page defines it)
- makes examples match the surrounding text
- keeps warnings and exceptions easy to spot
- gives each step of a multi-step walkthrough comparable depth — flag a
  step that is a bare command next to siblings with full explanations

### 3. Inspect code-adjacent text

For commands, YAML, shell snippets, and API examples:

- ensure the example is complete enough to run or adapt
- check that flags, paths, and release names are not stale
- verify the prose around the example explains what changes and why
- for any stated tool/version prerequisite, check it against what the
  workflow actually requires (e.g. a command uses a flag or operator
  that only exists from some minimum version) — a requirement that is
  present but wrong is worse than one that is simply missing
- check whether the page tells the reader what to do if the step fails,
  times out, or needs a retry, and whether failure-prone environment
  assumptions (network access, resource limits) are stated up front as
  prerequisites rather than discovered by trial and error

### 4. Report

Produce findings as a flat list, most severe first, one line each:

`[SEVERITY] EXPR-<n> — <file>:<line> — <problem, quoting the offending
text> — Fix: <one-line suggested rewording, or "needs judgment" if the
right wording depends on a decision the audit can't make>`

Group by severity:

- **HIGH** — text says the opposite of what the intended behavior is;
  an example is misleading enough to cause a bad operation; a stated
  prerequisite is wrong (not just unclear) and follows it fails.
- **MEDIUM** — ambiguous wording, unexplained jargon/reference, repeated
  terms for one concept, missing failure/retry guidance, or prose that
  obscures the actual workflow.
- **LOW** — awkward phrasing, overly long sentences, depth-parity
  drift, or style drift that does not change the meaning.

End with a short verdict for the page or doc set.

## Notes

- This skill is read-only; do not rewrite the page until the wording
  issue has been localized. Hand findings to [[fix-docs]] to apply them.
- Pair this with [[check-doc-structure]] so the page is both readable
  and well organized, and with [[check-doc-consistency]] so the prose
  matches the rest of the docs corpus.
- If the issue is a factual mismatch rather than wording, hand it off to
  [[check-doc-consistency]].
