---
name: standards
description: >
  Personal coding standards and conventions - stack defaults, KISS/YAGNI/DRY,
  responsive design, and security baseline. Use whenever writing, reviewing,
  or planning any code change, in any project, regardless of which other
  skill is active - for style, convention, architecture, or security guidance.
---

# Coding Standards

Reference only, no steps. Apply these whenever you write, review, or plan
code, whether this skill fired on its own or another skill (e.g.
`coding`, `code-review`) pulled it in.

## Stack defaults

- Runtime/package manager: **bun**. Use `bun`/`bunx` instead of `npm`/`npx`.
- Only impose this on a project that has no existing lockfile or tooling
  convention of its own — an existing codebase's choices win over this
  default.
- Skill installs go through `bunx skills add <repo>`.

## Design principles

**KISS** — simplest thing that works. 200 lines that could be 50 → rewrite.
A dependency replaceable with 10 clear lines → drop it.

**YAGNI** — build what's needed now. No config for cases that don't exist yet.
No abstraction for a single caller. No handling for states that can't occur.

**DRY, but not prematurely** — three similar lines are fine. Extract once a
third use clarifies what the actual abstraction is, not before.

**The Zen, as philosophy** — explicit over implicit, simple over complex,
flat over nested, readability counts, errors never pass silently, one
obvious way to do it.

## Patterns

You already know the standard patterns (MVC, composition over inheritance,
repository, etc.) and when each is textbook-appropriate. The risk isn't
missing knowledge, it's reaching for a familiar shape out of habit instead
of checking it against the problem in front of you. Before applying a
pattern, pause and ask: does this remove real complexity or duplication
here, or would the simpler thing work just as well? If you can't name what
it earns you, it's the wrong call — same bar as any abstraction.

## Comments

Default to no comments — well-named code already says what it does. Add
one only when it explains something the code can't: a non-obvious
constraint, a workaround for a specific bug, a hidden invariant. If
removing the comment wouldn't confuse a future reader, don't write it.

## Interfaces & UI

Every UI ships responsive by default, not bolted on afterward. Before
calling UI work done, check it at mobile (~375px), tablet (~768px), and
desktop (~1280px) widths.

## Security

*(Starter list — expand once uni material is reviewed.)*

You already know the standard vulnerability classes (OWASP Top 10,
injection, broken auth, secret leakage, etc.) — the risk is momentum:
writing input-handling, auth, or query code without pausing to check it
against what you already know. Before finishing anything that touches user
input, external data, secrets, or authentication, stop and run this list:

- Secrets, keys, or credentials never get committed — check `git status`/
  `git diff` before staging anything touching config or env files.
- External input is validated and sanitized; queries are parameterized,
  never built by string concatenation.
- API keys and tokens get least-privilege scopes, not broad ones "to be
  safe."

## Commits

Conventional Commits style (`feat:`, `fix:`, `refactor:`, ...). Use the
`caveman-commit` skill to produce the actual message text — don't hand-roll
commit wording here.
