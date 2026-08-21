# Source And Scope Rules

## Source Precedence

Use this precedence when resolving artifact content:

1. Explicit human request in the current refinement session.
2. Target artifact content and its existing structure.
3. Producer step instructions and gate instructions.
4. Read-only upstream artifacts under the original project `artifacts/` folder.
5. Read-only project `context-pack/` rules and constraints.
6. Clearly marked assumptions or open questions already present in the artifact.

If two authoritative sources conflict, do not silently choose one unless the
choice is directly supported by producer/gate instructions or the human request.
Make the conflict explicit in the artifact when the artifact has an existing
pending/open-question convention; otherwise explain it in the response.

## Read-Only Evidence Access

Allowed operations against original project sources:

- Read files.
- List directories.
- Search text.
- Inspect metadata needed to understand project structure.

Forbidden operations against original project sources:

- Write, patch, format, rename, delete, or generate files.
- Run commands that mutate original artifacts or context-pack content.
- Treat canonical paths as writable targets.

When a canonical artifact needs improvement, make the improvement in the
sandboxed target artifact. Stepwise applies approved files back later.

## Evidence Requirements

Evidence is required before changing:

- Scope, goals, user journeys, business rules, or acceptance criteria.
- Architecture, technology, runtime, security, infrastructure, or data claims.
- QE strategy, test obligations, coverage, priorities, or risk ratings.
- Dependencies, filenames, source paths, package names, commands, or versions.
- Status values, readiness decisions, blockers, and pending decisions.

Evidence may come from the current target artifact when the edit only clarifies
or restates content already present. For new factual content, search read-only
sources first.

## Handling Missing Evidence

When evidence is unavailable:

- Keep the unresolved item visible.
- Mark the item as pending, assumption, or open question using the artifact's
  existing pattern.
- Avoid confident wording.
- Mention the missing evidence in the response.

Do not convert uncertainty into a decision because the refinement needs to look
complete.
