# Argimlas skills

Personal agent skills for learning-focused coding assistance.

## Install

```sh
bunx skills add Argimlas/skills
```

## Skills

- `learn` — Collaborative learning mode. Guides you through concepts with a four-level hint ladder instead of solving problems outright. If a learner profile exists at `~/.learn-profile.md`, it uses that to tailor explanations to your background and assumes you're unfamiliar with anything not mentioned. Additional behaviors: reads files proactively after you say "done", suggests pseudocode before logic-heavy tasks, and does a quick sanity check after you mark something working.

  > **Note on skill priority:** When active, `learn` takes priority over interaction style — it controls how much gets revealed and at which level. Other skills (e.g. best-practices or coding workflow) still contribute domain knowledge; `learn` just controls the delivery. To avoid suppressing other skills in sessions where you don't want learning mode, the skill asks at the start of each session whether it should be active.

- `setup-learner` — One-time setup skill. Interviews you with a few questions and writes a personalized learner profile to `~/.learn-profile.md`. Run this once; the `learn` skill picks it up automatically in any future session.

> Run `/setup-learner` first, then use `/learn` as you code.

> **Tip:** `learn` handles the teaching style but not code planning. Pair it with a coding workflow skill (e.g. `coding-workflow` from [ValentinKolb/skills](https://github.com/ValentinKolb/skills)) to get disciplined, well-structured code alongside the explanations.

> **Note:** `~/.learn-profile.md` lives in your home directory and is never part of any project. Don't copy it into repos or commit it — it may contain personal context you wouldn't want to share.

### Legal skills

> [!WARNING]
> These skills are tailored to **German law** (DSGVO, BDSG, DDG, TDDDG, MStV) and are not suitable for other jurisdictions without expert review. They generate draft legal documents based on automated code analysis and **do not constitute legal advice**. Any output must be reviewed by a qualified lawyer or data protection officer before publication. Use for testing and prototyping only.

- `datenschutz` — Scans the codebase for data processing activities (cookies, analytics, APIs, forms) and produces an Art. 13 DSGVO-compliant Datenschutzerklärung in German, plus an inline compliance report. Integrates into the project's existing layout without adding CSS.

- `impressum` — Analyses the site's purpose, structure, and commercial nature to produce a minimal, correct Impressum under § 5 DDG and § 18 MStV. Flags outdated Impressums that still cite the replaced § 5 TMG.

- `cookie-banner` — Categorises every cookie and tracking technology by legal necessity under § 25 TDDDG and produces short, easy-to-read cookie-consent banner content in English with self-explanatory, category-specific buttons (e.g. "I accept analytics cookies"). Adds a responsive, centered banner layout while inheriting the project's existing colors and button styles.

## Recommended skills

- [ValentinKolb skills](https://github.com/ValentinKolb/skills) — disciplined coding workflow and concise documentation skills.
  ```sh
  bunx skills add ValentinKolb/skills
  ```
  `coding-workflow` uses Dex for task tracking. Install Dex and its skill as well:
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
