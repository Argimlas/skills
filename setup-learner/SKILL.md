---
name: setup-learner
description: >
  Sets up a personalized learner profile by interviewing the user and writing
  it to ~/.learn-profile.md. This profile is read by the learn skill to tailor
  hints and explanations. Use whenever the user says "set up my learner profile",
  "personalize the learn skill", "create my learning profile", "who am I as a
  learner", or wants to get more personalized help from the learn skill.
disable-model-invocation: true
---

# Setup Learner

Interview the user to build a personalized profile at `~/.learn-profile.md`.
The `learn` skill reads this file to tailor hints and explanations — the more
accurate it is, the better the learning experience.

## How to conduct the interview

Ask questions **one at a time**. Don't present a numbered list upfront — that
feels like a form, not a conversation. Let answers naturally flow into the
next question. Skip a question if the user has already answered it.

After all questions are answered (or the user says they're done), write the
file and confirm where it was saved.

## Questions to ask

1. **Background** — What's your programming experience? (e.g., languages,
   years, professional vs. hobby, what you've built)

2. **Strengths** — What do you know well enough that you don't need the basics
   explained? (e.g., "I know Python well", "comfortable with SQL and web APIs")

3. **Current focus** — What are you learning or building right now?

4. **Hint style** — When you're stuck, do you prefer a conceptual explanation
   first, or do you want to see a code example? (These aren't mutually exclusive
   — listen for nuance like "it depends on whether it's a syntax question or a
   concept question" and capture that faithfully.)

5. **Challenge level** — How much do you want to be pushed to figure things
   out yourself before getting the answer?

6. **Learning style** — Is there a way you wish concepts were introduced that
   would help you learn better? For example: do you want patterns named as they
   come up ("by the way, this is an MVC pattern"), analogies to things you
   already know, a heads-up when something is a common gotcha?

   If they say "not really" or "I'm not sure", prompt with: "Is there anything
   that's frustrated you in past learning experiences — something you wish had
   been explained differently?"

7. **Code quality** — Do you want examples and hints to always follow best
   practices, even if that makes them slightly more involved? (The reasoning:
   you can only build habits as good as what you're shown.)

Feel free to ask a natural follow-up if an answer is vague or interesting.

## Profile format

Write `~/.learn-profile.md` using this structure:

```markdown
# Learner Profile

## Background
[1–3 sentences summarizing experience and context]

## Knows well
- [Specific language, framework, or concept]
- [...]

## Currently learning
[What they're focused on right now]

## Preferences
- Hint style: [capture their actual answer, not a forced choice]
- Challenge level: [push me / balanced / just unblock me]

## Learning style notes
- [How they want concepts introduced — patterns named, analogies, gotcha warnings, etc.]
- Code quality: [always best practices / pragmatic / not specified]

## Gaps and notes
- [Concepts that keep coming up but feel shaky, or anything else worth knowing]
```

Keep each section concise — a few lines is enough. The goal is a quick
reference, not a biography.
