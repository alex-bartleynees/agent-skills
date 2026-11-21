---
name: manage-child-threads
version: "1.0.0"
description: Coordinate a builder and independent reviewers through child app threads in wiki-agents. Use when a manager agent delegates implementation, reviews a shared working tree, or sends fixes back for focused re-review.
---

# Manage child threads

Use child app threads for work that benefits from separate conversations, worker selection, or follow-up with the same agent. The manager owns the assignment, review baseline, and final result.

## 1. Assign the work

Use `thread_workers` to see eligible agents, their default provider instances, and provider readiness. Use `thread_worker_models` when the assignment needs a specific model. Supply explicit overrides only when the task calls for them. With no overrides, the child inherits the manager’s agent and provider instance and uses the applicable default model.

Use `thread_create` to start each assignment. Child app threads inherit the manager’s workspace and worktree, use their own autonomy, and cannot create child app threads. Launches and follow-ups are subject to the manager’s thread permission and approval flow.

Give each child a title and a prompt with:

- **Role and outcome** — what to build, review, or verify.
- **Scope and baseline** — workspace, relevant files, spec sources, and the exact comparison point if reviewing changes.
- **Permissions** — whether the child may edit, run gates, stage, or commit, according to the user’s authorization.
- **Report** — what evidence to return and what counts as completion.

Keep dependent work sequential. Give concurrent builders distinct files. When the builder and reviewers share a worktree, ask the builder to pause edits while reviewers inspect the snapshot. Independent read-only review assignments can run in parallel.

## 2. Collect implementation and review evidence

Ask the builder for changed files, verification commands and outcomes, and any checks that remain. Record the review baseline and whether the snapshot includes untracked files. Identify exclusions explicitly so every reviewer sees the same scope.

Assign review axes separately when independent judgments matter. Use the code-review skill for review method when available. Ask each reviewer for actionable findings with severity, file and line, impact, and a concrete correction, or an explicit clear result. For spec fidelity, have the reviewer cite the requirement. Put test and build execution with the builder when the review is read-only.

Use `thread_list` to check status, the manager’s child results to receive completed reports, and `thread_read` to recover a result. If the next step depends on a child, wait for its report or work on an independent task. When a child reports a blocking question, carry forward its role, decision context, and answer choices. Relay the user’s answer to that same child with `thread_send`.

## 3. Close the review loop

Send accepted findings to the builder with the behavior to fix and any regression the fix should cover. Once the builder reports the revised snapshot and verification results, use `thread_send` to return each finding to its original reviewer. Ask for closure against the revised code and a check of any behavior the fix could affect.

For example, if a provider call held a shared lock, ask the reviewer to trace the new lock boundary and inspect regression tests that stall the provider. Check both that unrelated work can proceed and that operations that share the lock still serialize correctly.

Finish when each requested deliverable is accounted for and each remaining finding is resolved or explicitly reported as open. Summarize what changed, the final verdict for each review axis, and the verification actually performed. Attribute builder-run gates and reviewer conclusions to their reports.
