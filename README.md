# Argimlas skills

Personal agent skills for disciplined coding, learning-focused assistance, and German legal-document generation (Datenschutzerklärung, Impressum, cookie banners).

## Install

```sh
bunx skills add Argimlas/skills
```

## Skills

### Learning skills

- `learn` — Collaborative learning mode. Guides you through concepts with a four-level hint ladder instead of solving problems outright. If a learner profile exists at `~/.learn-profile.md`, it uses that to tailor explanations to your background and assumes you're unfamiliar with anything not mentioned. Additional behaviors: reads files proactively after you say "done", suggests pseudocode before logic-heavy tasks, and does a quick sanity check after you mark something working.

  > **Note on skill priority:** When active, `learn` takes priority over interaction style — it controls how much gets revealed and at which level. Other skills (e.g. `standards` or `coding`) still contribute domain knowledge; `learn` just controls the delivery. To avoid suppressing other skills in sessions where you don't want learning mode, the skill asks at the start of each session whether it should be active.

- `setup-learner` — One-time setup skill. Interviews you with a few questions and writes a personalized learner profile to `~/.learn-profile.md`. Run this once; the `learn` skill picks it up automatically in any future session.

> Run `/setup-learner` first, then use `/learn` as you code.

> **Tip:** `learn` handles the teaching style but not code planning. Pair it with a coding workflow skill — this repo's own `coding`, or [ValentinKolb/skills](https://github.com/ValentinKolb/skills)' `coding-workflow` — to get disciplined, well-structured code alongside the explanations. Use one or the other, not both.

> **Note:** `~/.learn-profile.md` lives in your home directory and is never part of any project. Don't copy it into repos or commit it — it may contain personal context you wouldn't want to share.

### Coding skills

- `standards` — Coding standards and conventions: stack defaults (bun), KISS/YAGNI/DRY/Zen, patterns, comments, responsive UI, a starter security baseline, and commit-message convention. Model-invoked, so other skills (including `code-review`) pull it in automatically for style, convention, or security guidance.

- `coding` — Disciplined coding workflow: scan the codebase, ask before assuming, propose alternatives for non-trivial decisions, plan tasks in a committable checklist, execute against `standards`, and wrap up with review. User-invoked only. Structure and core principles adapted from ValentinKolb/skills' `coding-workflow` (MIT; see `THIRD_PARTY_NOTICES.md`).

### Legal skills

> [!WARNING]
> These skills are tailored to **German law** (DSGVO, BDSG, DDG, TDDDG, MStV) and are not suitable for other jurisdictions without expert review. They generate draft legal documents based on automated code analysis and **do not constitute legal advice**. Any output must be reviewed by a qualified lawyer or data protection officer before publication. Use for testing and prototyping only.

- `datenschutz` — Scans the codebase for data processing activities (cookies, analytics, APIs, forms) and produces an Art. 13 DSGVO-compliant Datenschutzerklärung in German, plus an inline compliance report. Integrates into the project's existing layout without adding CSS.

- `impressum` — Analyses the site's purpose, structure, and commercial nature to produce a minimal, correct Impressum under § 5 DDG and § 18 MStV. Flags outdated Impressums that still cite the replaced § 5 TMG.

- `cookie-banner` — Categorises every cookie and tracking technology by legal necessity under § 25 TDDDG and produces short, easy-to-read cookie-consent banner content in English with self-explanatory, category-specific buttons (e.g. "I accept analytics cookies"). Proposes a consent-expiry period (12 months by default, per DSK guidance) and confirms it with you before writing the consent cookie. Adds a responsive, centered banner layout while inheriting the project's existing colors and button styles.

## Recommended skills

- [ValentinKolb skills](https://github.com/ValentinKolb/skills) — disciplined coding workflow and concise documentation skills.
  ```sh
  bunx skills add ValentinKolb/skills
  ```
  Its `coding-workflow` covers the same ground as this repo's own `coding` skill — `coding` is in fact adapted from it (see `THIRD_PARTY_NOTICES.md`). Install one or the other, not both. `coding-workflow` uses Dex for task tracking; `coding` uses a plain markdown checklist instead, so only add Dex if you go with Valentin's version:
  ```sh
  bun add -g @zeeg/dex
  bunx skills add dcramer/dex
  ```

- [juliusbrussee caveman](https://github.com/juliusbrussee/caveman) — compresses agent output ~65% using terse, fragment-based language without losing technical accuracy.
  ```sh
  bunx skills add juliusbrussee/caveman@caveman-commit
  ```

- [Anthropic skill-creator](https://github.com/anthropics/skills/tree/main/skills/skill-creator) — create, iterate, and evaluate agent skills with test runs and a built-in benchmark viewer.
  ```sh
  bunx skills add anthropics/skills@skill-creator
  ```

- [Vercel Labs find-skills](https://github.com/vercel-labs/skills/tree/main/skills/find-skills) — discover and install new skills interactively when you don't know what's available.
  ```sh
  bunx skills add vercel-labs/skills@find-skills
  ```

- [Matt Pocock code-review](https://github.com/mattpocock/skills) — reviews code changes for bugs, style, and cleanup opportunities.
  ```sh
  bunx skills add mattpocock/skills@code-review
  ```
