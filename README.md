# Argimlas skills

Personal agent skills for learning-focused coding assistance.

## Install

```sh
bunx skills add Argimlas/skills
```

## Skills

- `learn` — Collaborative learning mode. Guides you through concepts with a four-level hint ladder instead of solving problems outright. If a learner profile exists at `~/.learn-profile.md`, it uses that to tailor explanations to your background and assumes you're unfamiliar with anything not mentioned.

- `setup-learner` — One-time setup skill. Interviews you with a few questions and writes a personalized learner profile to `~/.learn-profile.md`. Run this once; the `learn` skill picks it up automatically in any future session.

> Run `/setup-learner` first, then use `/learn` as you code.

> **Tip:** `learn` handles the teaching style but not code planning. Pair it with a coding workflow skill (e.g. `coding-workflow` from [ValentinKolb/skills](https://github.com/ValentinKolb/skills)) to get disciplined, well-structured code alongside the explanations.

> **Note:** `~/.learn-profile.md` lives in your home directory and is never part of any project. Don't copy it into repos or commit it — it may contain personal context you wouldn't want to share.

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
