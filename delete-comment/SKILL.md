---
name: delete-comment
version: "1.0.0"
description: Audit and remove unnecessary source code comments in reviewable batches. Use when asked to audit or clean up code comments, or when invoked as delete-comment.
---

# Delete comment

## Inventory

Use the requested file or directory. If no path is given, inventory the repository's configured comment-lint scope or, when there is no configured scope, its first-party source. Pick a coherent batch and report the remainder.

Read the repository instructions and check for a comment inventory command, lint baseline, issue tracker, and decision records. Prefer the repository's inventory tool. Otherwise, use a language-aware parser or inspect source manually. Treat text search as candidate discovery and confirm each span is a comment before editing.

For each candidate, read the surrounding implementation and relevant tests. Check history or documentation when needed to understand why a comment exists.

## Decide what to remove

| Category | Action |
| --- | --- |
| Tautology | Remove wording that merely restates what the implementation makes clear. |
| Excuse | Verify the underlying debt and link an existing issue or file one when authorized. Capture the impact and a concrete completion criterion. Keep the comment if the debt cannot yet be durably recorded. |
| Stale | Confirm the premise no longer applies before removing the comment. |
| Decision record | Preserve the current constraint, decision, and consequence in the repository's established ADR or design documentation. Remove the inline comment once that record is in place and the context remains easy to find. |

Keep comments that carry useful local context and comments with operational or legal meaning. Examples include compiler and linter directives, generated-file markers, license notices, and public API documentation consumed by tooling. Leave uncertain and unreviewed comments in place.

## Edit and verify

Limit source edits to comment removal and whitespace needed to preserve syntax, token boundaries, and behavior. Check inline and multiline removals for changes to indentation or newline-sensitive syntax. Review the diff and run the relevant repository checks.

If a comment lint baseline exists, use the repository's supported maintenance workflow and verify that every reduction corresponds to a comment removed in this batch. Preserve entries for everything outside the reviewed removals.

Report removal counts by category, decision records and issues, checks run, and pending scope. Count physical lines removed only when they contained comment text or delimiters and no code. Count each line once. Report potential savings separately if the request was audit-only.
