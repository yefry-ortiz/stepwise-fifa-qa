---
name: refining-artifact-consistency
description: >
  Governs interactive refinement of AI Pods artifacts inside Stepwise refinement
  sandboxes. Applies source fidelity, zero invention, structure preservation,
  surgical edits, downstream contract safety, and concise rationale across
  human-in-the-loop artifact refinement sessions.
license: Proprietary
metadata:
  author: aipods-team
  version: 1.2.0
  category: meta
  tags: refinement, artifact-governance, quality-gate, source-fidelity, stepwise
---

# Refining Artifact Consistency

## Quick Start

Use this skill when a human asks for changes during a Stepwise artifact
refinement session. The agent may read the complete original `artifacts/` and
`context-pack/` folders as evidence, but must write only to the sandboxed target
artifact files approved by Stepwise.

The goal is not to regenerate the artifact. The goal is to make the smallest
coherent, evidence-backed edit that improves the artifact while preserving the
contracts downstream steps depend on.

## Operating Modes

### Embedded Stepwise refinement mode

This is the normal mode. Stepwise owns lifecycle state, transcripts, diffs,
sandbox setup, approval, and apply-back. In this mode:

- Do not create `_progress.json`.
- Do not run Stepwise completion or state commands.
- Do not edit canonical project files.
- Edit only the writable sandbox target artifacts listed in the prompt.
- Use read-only original artifacts and context-pack folders only for read, list,
  and search operations.
- For multi-turn sessions, maintain a `REFINEMENT-NOTES.md` working-set note at
  the sandbox root (outside the target artifacts, so it is never applied back).
  On the second and later turns, read it first and recall instead of re-reading
  the full sources each turn. See `references/working-set-protocol.md`.

### Standalone audit mode

Use only if a direct skill execution asks for an audit of a proposed refinement
without Stepwise session management. In that case, write an audit file to the
requested output path and do not modify source artifacts unless the caller
explicitly provides a writable target path.

## Inputs

| Input | Required | Purpose |
|-------|----------|---------|
| `sandbox_target_artifacts` | Yes | Writable artifact files or folders copied by Stepwise into the refinement sandbox. |
| `read_only_artifacts_path` | Yes | Original project artifact tree used as canonical evidence. |
| `read_only_context_pack_path` | No | Project rules, architecture context, and execution constraints. |
| `human_request` | Yes | The refinement request or quick-action instruction. |
| `producer_instructions` | No | Original generator step instructions that define expected artifact shape. |
| `gate_instructions` | No | Human or automated gate criteria that triggered refinement. |
| `selected_text` | No | Text selection from the rendered artifact view, when present. |

## Core Contract

1. Source fidelity wins. Load the target artifact and relevant read-only sources
   before changing factual, architectural, scope, dependency, or traceability
   claims.
2. Zero invention. If evidence is missing or contradictory, do not fabricate a
   resolution. Preserve or add an explicit pending item, assumption, or
   `open_questions` entry according to the artifact's existing structure.
3. Preserve downstream contracts. Keep stable IDs, section names, heading
   hierarchy, tables, schemas, references, filenames, status values, and
   traceability links unless the human explicitly asks for a contract change.
4. Make surgical edits. Change only the lines or sections needed to satisfy the
   request. Avoid whole-document rewrites.
5. Keep canonical sources read-only. Never create, edit, delete, rename, format,
   or patch files under the original project root, original `artifacts/`, or
   original `context-pack/`.
6. Explain briefly. The final response must list changed sandbox files, the
   rationale/evidence used, and unresolved items.

## Workflow

### Step 1: Orient

Read the Stepwise refinement prompt and identify:

- Writable sandbox target artifacts.
- Read-only original artifact and context-pack paths.
- The active file or selected text, if any.
- Producer and gate expectations.
- Whether the request is clarification, consistency validation, pending
  resolution, selection refinement, or search.

If writable targets or access rules are missing, ask one concise clarification
before editing.

### Step 2: Load the Minimum Evidence

On a resume turn of a multi-turn session, read `REFINEMENT-NOTES.md` first and
recall what was already established; do not re-read sources the note's evidence
map already covers (see `references/working-set-protocol.md`).

Read the active target artifact first. Then search or read only the relevant
read-only sources needed for the requested change:

- For terminology or scope conflicts, search upstream PRD, epics, stories,
  architecture, ADR, domain, QE, and code-design artifacts.
- For pending decisions, search context-pack and upstream artifacts before
  resolving.
- For generated code task or implementation plans, verify referenced source
  paths, package names, runtime constraints, and test obligations against
  original artifacts or context-pack.

Do not bulk-load unrelated files when a targeted search answers the question.

### Step 3: Classify the Edit

Use one of these edit classes:

- `clarification`: Adds context, definitions, or rationale already supported by
  the artifact or read-only sources.
- `consistency-fix`: Aligns contradictory statements, totals, statuses, names,
  or references.
- `pending-resolution`: Resolves a TBD, placeholder, assumption, or open
  question with evidence.
- `selection-refinement`: Improves selected rendered text while preserving local
  structure.
- `search-only`: Reports related context and edits only if the fix is obvious,
  localized, and evidence-backed.

### Step 4: Apply Surgical Changes

Before writing, check:

- The destination is a sandbox target artifact.
- The change preserves the artifact's expected format.
- The change is grounded in target artifact text or read-only evidence.
- Any unresolved item remains explicit instead of hidden.

Prefer editing the smallest coherent block. Preserve existing wording style,
table shape, list conventions, ID prefixes, and section ordering.

**Tool discipline — apply `execution-protocol.md` §10.5 + §10.5.1 (mandatory before any sandbox write).**

- **Default tool:** the harness's targeted in-place-edit operation (`Edit` in Claude Code; `edit` in other harnesses). One call per surgical change, with surrounding context in `old_string` to make the match unique.
- **Non-ASCII fallback (§10.5.1, TEMPORARY).** If the target artifact contains non-ASCII codepoints (Spanish / French / Portuguese / German / any accented vowel, em dash, en dash, smart quotes, curly apostrophes, …), or the `new_string` you are about to write contains any of those characters, **use the harness's full-file replace operation instead of targeted-edit**:
  1. Read the current sandbox target file from disk.
  2. Compose the surgically-edited full file content in memory (preserve every non-changed byte verbatim).
  3. Replace the file via the full-file write operation (`Write` in Claude Code; `replace` in other harnesses).
  4. Drop the composed content from working memory.

  Reason: the runtime's targeted-edit operation has been observed to fail with `'ascii' codec can't encode character …` errors **AND** truncate the target file to 0 bytes on encoding failure (atomic-write contract violated). Until the runtime is patched, full-file replace is the only safe path for non-ASCII content.

- **NEVER** use `Bash + sed / awk / python3 <<EOF / cat >` to mutate sandbox target files. Those bypass the harness's edit validation, defeat surgical-edit discipline, and risk corrupting the artifact.
- **If a write call returns an error**, fix the call (verify the path, the `old_string` match, that the destination is a writable sandbox target). Do NOT switch to `Bash`/`python3` as a workaround — that path has been observed to cascade into bulk rewrites that violate sandbox isolation.
- **Detection shortcut:** if you are unsure whether the artifact contains non-ASCII, default to the §10.5.1 full-file replace path. The cost is one extra read; the cost of getting it wrong is a zeroed-out sandbox artifact.

### Step 5: Validate the Result

After editing, reread the changed section and verify:

- No unsupported facts were introduced.
- IDs, headings, cross-references, and status values still make sense.
- Existing pending/open question conventions are preserved.
- The change does not require edits outside writable sandbox targets.

### Step 6: Respond

Keep the response concise:

- Changed files.
- What was changed and why.
- Evidence used, naming the source path or section when useful.
- Remaining unresolved items or assumptions.

Do not claim that Stepwise approval or apply-back has happened; Stepwise handles
diff review and apply-back after the human approves.

## Quick-Action Behavior

Read `references/quick-action-guidance.md` for detailed quick-action behavior.
The short rules are:

- Clarify section: add supported definitions and rationale; do not invent.
- Validate consistency: fix clear contradictions; report unresolved conflicts.
- Resolve pendings: search read-only evidence; unresolved stays pending.
- Refine selection: preserve local structure, IDs, and surrounding meaning.
- Search selection: report findings; edit only obvious localized defects.

## Reference Files

| File | When to Read |
|------|--------------|
| `references/source-and-scope.md` | Before resolving facts, scope, access, or source-of-truth questions. |
| `references/edit-protocol.md` | Before applying non-trivial edits or changing structured artifact content. |
| `references/quick-action-guidance.md` | When the request came from a refinement quick-action button or selected text. |
| `references/working-set-protocol.md` | On the second and later turns of a multi-turn session, to recall instead of re-reading. |

## Anti-Patterns

| Anti-pattern | Why it fails |
|--------------|--------------|
| Regenerating the full artifact for a small request | It breaks reviewability and can damage downstream traceability. |
| Editing original project artifacts from inside refinement | It bypasses Stepwise diff approval and violates sandbox isolation. |
| Resolving TBDs from plausible guesses | It hides uncertainty and creates false project knowledge. |
| Renaming IDs, headings, schemas, or filenames casually | Downstream capabilities may bind to those contracts. |
| Adding generic rationale without evidence | It creates noise and weakens quality-gate trust. |
| Asking broad questions before searching available context | The refinement agent has enough read-only evidence access to investigate first. |
| Re-reading the full target and upstream sources every turn of a multi-turn session | Each re-read inflates the context re-sent on all later turns; recall from `REFINEMENT-NOTES.md` instead. |
| Spending a sandbox turn on a trivial cosmetic edit without committing to it | A discarded cosmetic refinement burns a turn for nothing; make the one surgical edit directly. |

## Completion Criteria

A refinement turn is complete when:

- Requested changes were applied only to sandbox targets, or no safe edit was
  possible.
- The agent response identifies changed files and remaining unresolved items.
- Unsupported changes were avoided.
- The target artifact remains structurally compatible with producer and gate
  expectations.
