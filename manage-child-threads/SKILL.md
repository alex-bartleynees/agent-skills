---
name: manage-child-threads
version: "1.0.0"
description: Coordinate a builder and independent reviewers through child app threads in wiki-agents. Use when a manager agent delegates implementation, reviews a shared working tree, or sends fixes back for focused re-review.
---

# Manage child threads

Use child app threads for work that benefits from separate conversations, worker selection, or follow-up with the same agent. The manager owns the assignment, review baseline, and final result.

## 1. Assign the work

When exposed, use `thread_workers` to check eligible agents, default provider instances, and provider readiness. Use `thread_worker_models` when the assignment requires a specific model and the tool is exposed. Supply overrides only when the assignment calls for them. Without overrides, the child inherits the manager’s agent and provider instance and uses the applicable default model. On older toolkits, use those defaults when they satisfy the assignment. If a required worker or model cannot be verified, report the blocker.

Use `thread_create` to start each assignment. Child app threads inherit the manager’s workspace and worktree, use their own autonomy, and cannot create child app threads. Launches and follow-ups are subject to the manager’s thread permission and approval flow.

Give each child a title and include these details in its prompt.

- **Role and outcome** — what to build, review, or verify.
- **Scope and baseline** — workspace, relevant files, spec sources, and the exact comparison point if reviewing changes.
- **Permissions** — whether the child may edit, run gates, stage, or commit, according to the user’s authorization.
- **Report** — what evidence to return and what counts as completion.

Keep dependent work sequential. Give concurrent builders distinct files. When the builder and reviewers share a worktree, ask the builder to pause edits while reviewers inspect the snapshot. Independent read-only review assignments can run in parallel.

## 2. Collect implementation and review evidence

Ask the builder for changed files, verification commands and outcomes, and any checks that remain. Record the review baseline and whether the snapshot includes untracked files. Identify exclusions explicitly so every reviewer sees the same scope.

Assign review axes separately when independent judgments matter. Use the `code-review` skill for review methodology when available. For each review assignment, record the child ID, turn ID when available, review axis, comparison baseline, and snapshot under review. Record the revised snapshot for each follow-up. Count a report as clearance only when it answers that assignment on the intended snapshot. A skill-design reply in a reused review thread leaves the code review pending. Put test and build execution with the builder when the review is read-only.

Use `thread_list` to check status and the manager’s child results to receive completed reports. `thread_read` retrieves the child’s latest finished turn. Match it to the assignment before using it as review evidence. If the next step depends on a child, wait for its report or work on an independent task.

`thread_send` accepts a child only when it is idle and has no active subagents. A finished child can appear in `thread_list` as `done`, `failed`, or `stopped`. If the child or its subagents are still active, retain the follow-up and send it once the child is idle and its subagents have finished. When a child reports a blocking question, carry forward its role, decision context, and answer choices. Relay the user’s answer to the same child when it can receive the follow-up.

## 3. Close the review loop

Send accepted findings to the builder with the behavior to fix and any regression the fix should cover. Once the builder reports the revised snapshot and verification results, use `thread_send` to return each finding to its original reviewer. Ask for closure against the revised code and a check of any behavior the fix could affect.

Before reporting completion, confirm that required review findings are closed and required gates passed on the final snapshot. Complete any authorized delivery steps, such as committing. If a blocker remains, report the exact finding, gate, or delivery step and its status. Summarize what changed, the final verdict for each review axis, and the verification performed. Attribute builder-run gates and reviewer conclusions to their reports.
