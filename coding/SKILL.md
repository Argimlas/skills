---
name: coding
description: >
  My personal disciplined coding workflow - scan the codebase, propose
  alternatives, plan tasks in a committable markdown file, execute against
  standards, and wrap up with review. Invoke by name when starting
  non-trivial code work.
disable-model-invocation: true
---

# Coding Workflow

Work through these phases in order. No phase skipping, no coding before an
explicit go. Apply `standards` throughout — don't restate its rules
here, just follow them.

## Phase 0 — Scan

Read the files this task will touch or call into, plus enough surrounding
structure to understand how the project is built.

Skip this phase entirely for an empty/new project — go straight to Phase 2.

**Done when:** you can state, in a few sentences, the project's structure,
where similar logic already lives, and any existing convention that touches
this task — without needing to read further.

## Phase 1 — Mode

Ask once, at the start of this chat: *"Autonomous — I commit, push, and run
the review myself — or manual — I hand you commit messages and you drive
git?"* Don't ask again this chat once answered, even across multiple tasks.

## Phase 2 — Understand & Propose

Restate the task in your own words. Ask clarifying questions on anything
you'd otherwise have to assume — assume as little as possible, better to ask.

If the task is small and bounded — a one-line fix, a rename, a single
obvious change — state the approach and why, then move on. Reserve the full
comparison below for decisions that are actually open.

Otherwise, present **3 genuinely different approaches**, not variations of
one idea. Each: 3-6 bullets — core idea, tradeoffs, main pitfalls. End with
your recommendation and why.

**Done when:** the user has picked a direction, or explicitly told you to
proceed with your recommendation.

## Phase 3 — Task Plan

Turn the chosen approach into a `.coding-tasks.md` checklist (repo
root, unless the user says otherwise) — concrete, verifiable steps, each
with a success criterion. Commit this file: it's what lets the user resume
the same work from a different device.

**Done when:** a senior engineer could predict the diff from the checklist
alone.

Wait for an explicit "go" before writing any code.

## Phase 4 — Execute

Work through `.coding-tasks.md` top to bottom.

- Surgical changes only — every changed line traces to the current task.
  Don't improve adjacent code.
- Check a task's box only once it's verified (tests pass, build succeeds,
  behavior confirmed) — "should work" doesn't count.
- After finishing a task, or a small cluster of related ones, give the user
  a short manual test checklist in chat when there's something worth a
  human eyeballing — concrete actions ("click X", "check the mobile
  viewport at ~375px", "reload and confirm Y persists"), not a vague "please
  test this."
- **Autonomous mode:** commit after each completed task or cluster. Push
  only if already on a feature/working branch (not `main`/`master`) — if the
  checked-out branch is main, commit locally and ask the user before ever
  pushing.
- **Manual mode:** call `caveman-commit` to produce the message, hand it to
  the user, let them commit and push themselves.

On a failed test/build or an unexpected error: read the error, form one
hypothesis, test it with the smallest change. If wrong, try a genuinely
different hypothesis — not a variation of the same fix. After 3 distinct
attempts without success, stop and report what you tried and what you
believe is going on, rather than continuing to guess.

**Done when:** every task in `.coding-tasks.md` is checked and verified.

## Phase 5 — Wrap-up

Delete `.coding-tasks.md` once every task is checked — it served
its purpose, git history is the record now.

**Autonomous mode only:** run the `code-review` skill against
`standards` before considering the work finished.
