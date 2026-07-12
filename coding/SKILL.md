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

Work through these phases in order. Apply `standards` throughout — don't
restate its rules here, just follow them.

## Phase 0 — Scan

Read the files this task will touch or call into, plus enough surrounding
structure to understand how the project is built.

Skip this phase entirely for an empty/new project — go straight to Phase 2.

**Done when:** you can state, in a few sentences, the project's structure,
where similar logic already lives, and any existing convention that touches
this task — without needing to read further.

## Phase 1 — Mode

Ask once, at the start of this chat: *"Autonomous — I commit after each
task myself — or manual — I hand you a commit message after each task and
you commit it yourself?"* Don't ask again this chat once answered, even
across multiple tasks.

This choice governs only who writes the commit in Phase 4 onward — pushing
stays the user's call in both modes, never yours. Phases 0-3 — Scan,
Propose, Task Plan — always run in full and always stop for the user's
input, in both modes; autonomous never means skipping ahead without
checking in.

## Phase 2 — Understand & Propose

Restate the task in your own words. Ask clarifying questions on anything
you'd otherwise have to assume — assume as little as possible, better to
ask. Treat this as a back-and-forth: a rough idea rarely resolves in one
round, so let each answer surface the next question until the shape of the
task is clear.

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
with a success criterion. Write it as an actual file with the Write tool and
commit it: it's what lets the user resume the same work from a different
device. The in-chat task tracker doesn't survive a session end or a device
switch — it's not a substitute for this file.

**Done when:** a senior engineer could predict the diff from the checklist
alone.

Wait for an explicit "go" before writing any code.

## Phase 4 — Execute

Work through `.coding-tasks.md` top to bottom.

- Surgical changes only — every changed line traces to the current task.
  Leave adjacent code alone. If you spot something unrelated — a bug, a
  mismatch — note it and save it for the Phase 5 review instead of fixing
  or asking about it mid-task.
- Check a task's box only once it's verified with what's already in the
  project — existing tests pass, the build succeeds — "should work" doesn't
  count. Don't install new tooling (e.g. a browser automation stack) to
  self-verify.
- After finishing a task, or a small cluster of related ones, give the user
  a short manual test checklist in chat for anything you can't verify with
  what's already there — concrete actions ("click X", "check the mobile
  viewport at ~375px", "reload and confirm Y persists"), not a vague "please
  test this."
- **Autonomous mode:** if not already on a feature/working branch, create
  one and switch to it before touching any code — never work directly on
  `main`/`master`. Commit after each completed task or cluster; don't push.
- **Manual mode:** at that same point, call `caveman-commit` to produce the
  message, hand it to the user, let them commit and push themselves.

On a failed test/build or an unexpected error: read the error, form one
hypothesis, test it with the smallest change. If wrong, try a genuinely
different hypothesis — not a variation of the same fix. After 3 distinct
attempts without success, stop and report what you tried and what you
believe is going on, rather than continuing to guess.

**Done when:** every task in `.coding-tasks.md` is checked and verified, and
`git diff` shows no line outside those tasks' scope.

## Phase 5 — Wrap-up

Give the user a short review in chat, regardless of mode: what was done,
what problems came up (failed attempts, workarounds, deviations from the
plan), any unrelated bugs or mismatches spotted along the way but left
untouched, and anything else worth flagging.

**Autonomous mode:** fold every manual test checklist item from Phase 4 into
one consolidated list here — this is the user's checkpoint before they
decide to push. Pushing is never yours to do, in either mode.

Delete `.coding-tasks.md` once every task is checked — it served
its purpose, git history is the record now.

**Done when:** the review — and, in autonomous mode, the consolidated
checklist — has been posted.
