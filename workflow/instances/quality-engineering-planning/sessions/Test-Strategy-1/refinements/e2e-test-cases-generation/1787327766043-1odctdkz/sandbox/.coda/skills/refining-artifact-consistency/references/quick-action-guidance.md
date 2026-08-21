# Quick-Action Guidance

## Clarify Section

Purpose: make a selected section understandable to downstream reviewers without
changing its meaning.

Do:

- Expand acronyms, roles, inputs, outputs, and rationale already supported by
  the artifact or read-only evidence.
- Add concise context inside the existing section shape.
- Preserve IDs, acceptance criteria, references, status values, and ordering.

Do not:

- Add new scope or decisions without evidence.
- Convert assumptions into facts.
- Rewrite the whole section for style only.

## Validate Consistency

Purpose: find and fix contradictions inside the target artifact and against
read-only upstream evidence.

Check:

- Conflicting totals, statuses, dates, names, priorities, or risk ratings.
- Mismatched terminology across sections.
- Stale references to earlier decisions.
- Traceability links that point to missing or inconsistent IDs.
- Scope drift against producer/gate instructions.

Fix only clear inconsistencies. If sources disagree, keep the conflict visible
and summarize the unresolved decision in the response.

## Resolve Pendings

Purpose: resolve placeholders, TBDs, assumptions, weak decisions, and open
questions when evidence exists.

Process:

1. Search the target artifact for pending markers.
2. Search read-only artifacts and context-pack for evidence.
3. Replace the pending item only when the resolution is directly supported.
4. Keep unsupported items pending and explain what evidence is missing.

Do not resolve pending decisions from plausibility alone.

## Refine Selection

Purpose: improve selected rendered text in place.

Rules:

- Edit the local selection or its smallest containing block.
- Preserve section structure, IDs, tables, and nearby references.
- Keep the original intent unless the human asks for a content change.
- Use read-only evidence before adding facts.

## Search Selection

Purpose: find related context, duplicates, or inconsistencies for selected text.

Default behavior is read-only analysis. Edit only when the fix is obvious,
localized, and evidence-backed, such as correcting a stale repeated status or
aligning a duplicated term.

The response should name the files or sections searched and the relevant
findings.
