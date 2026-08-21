# Edit Protocol

## Pre-Edit Checklist

Before writing:

- Confirm the destination path is one of the writable sandbox targets.
- Reread the section that will change.
- Identify whether the change affects a downstream contract.
- Locate evidence for any new factual claim.
- Decide the smallest coherent block to edit.

If a requested change requires modifying a non-target file, explain that the file
is outside the writable scope and ask the human to include it as a target
artifact.

## Structure Preservation

Preserve these unless the human explicitly requests a contract change:

- File names and relative paths.
- Top-level artifact type and declared metadata.
- Stable IDs, prefixes, and numbering schemes.
- Heading hierarchy and section names.
- Table columns and schema-like fields.
- Traceability links to FR, NFR, AC, epic, story, ADR, domain, test, or task IDs.
- Output manifests, ledgers, indexes, and status fields.

If a section must move, keep its IDs and links intact and mention the movement in
the response.

## Surgical Editing

Use the smallest edit that satisfies the request:

- Prefer line or paragraph replacement over section rewrite.
- Prefer filling specific pending fields over creating new parallel sections.
- Prefer adding a compact note inside an existing review/change-log section over
  inventing a new document convention.
- Preserve existing markdown style and ordering.

Avoid broad copy edits during consistency or pending-resolution tasks unless the
human explicitly asks for writing cleanup.

## Artifact Change Notes

If the human asks the document itself to carry downstream review context, append
or update an existing "changes", "review notes", "refinement notes", or
equivalent section. If no convention exists, add a compact final section such as
`## Refinement Notes` only when it does not violate the artifact schema.

Each note should include:

- What changed.
- Why it changed.
- Evidence or source used.
- Remaining pending items.

Keep notes factual and concise. Do not include chat transcript noise.

## Post-Edit Validation

After writing:

- Reread the changed section.
- Check nearby references for stale wording.
- Search for old values when replacing terminology or statuses.
- Verify no new unsupported claim was introduced.
- Verify no canonical source path was changed.

If validation reveals a broader issue outside writable scope, leave the target
artifact consistent where possible and report the remaining issue.
