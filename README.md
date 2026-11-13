# designloop

An agent skill that raises the quality of an existing interface, or its writing, by **measuring it, not guessing**.

Each round the agent:

1. **Captures** the real app the same way (stills per theme and state, short clips for motion).
2. **Judges** it with *fresh* subagents against a fixed, weighted rubric — nitpicky, and regressions score lower.
3. **Fixes** what the judges found, using separate implementer subagents.
4. **Repeats** until a target score or round limit is hit — e.g. `5.0 → 6.75 → 8.5 → 9.15`.

Judges never implement, implementers never judge, judges are new every round, and the capture set never changes — so the scores stay comparable and honest.

## Three modes

- **Professional polish** — one set of tokens everywhere, fewer type sizes, a consistent spacing scale, contrast, hit targets, state coverage, light/dark parity, motion quality. The target is the app's own best self.
- **Art direction** — move the UI toward a specific look: a named style, reference sites/apps, a generated target image, or a Figma design to match (exactly or as a guide).
- **Writing** — the same loop for words: UI copy (buttons, errors, empty states, onboarding), docs and READMEs, landing pages, posts. Judges score clarity, concision, voice, consistency and accuracy for a named reader, and a fresh *reader* agent reads each text cold to show where it confuses. Meaning stays fixed; the words get better.

## Install

**Claude Code (plugin):**

```
/plugin marketplace add mubarakmarafa/designloop
/plugin install designloop@designloop
```

**Claude Code (manual):**

```sh
git clone https://github.com/mubarakmarafa/designloop.git
cp -R designloop/skills/designloop ~/.claude/skills/
```

**Other agents** (Codex, etc.): copy `skills/designloop/` wherever your agent loads skills from. It's plain Markdown — `SKILL.md` plus `references/`.

## Use

In a project with a UI you can run, say something like:

- `/designloop tighten the chrome — it feels inconsistent`
- `/designloop make it look like graphic realism, neon pop with brutalist layouts`
- `/designloop match this Figma: <link>`
- `/designloop rewrite all our error messages and empty states`
- `/designloop tighten the README and the getting-started docs`

It interviews you first, studies how your project runs and ships, then builds a repeatable capture harness and starts the loop. By default it delivers a branch and an **unmerged** PR. It never merges or releases unless you ask.

**Cost note:** each round spawns several subagents. Set a round cap when it asks for a stop condition.

## What it asks first, and why it matters

Before any agent touches your code, designloop asks a short set of questions, using multiple-choice prompts with a recommended option. It skips anything your request already answered. It writes the answers to `.designloop/BRIEF.md` and `RUBRIC.md`, and every judge and implementer reads those files. So each answer has a direct effect on how the loop runs.

### 1. Mode

Polish, art direction or writing. It's usually inferred from your wording ("tighten" or "inconsistent" means polish; "make it look like" or a Figma link means art direction; "copy", "docs" or "error messages" means writing).

**Effect:** picks the rubric, the extra questions and how much change is allowed.
- **Polish** keeps your look and counts inconsistencies.
- **Art direction** turns a style into 5–8 traits a judge can point to, then pauses after round 1 so you can approve the new look before it spreads.
- **Writing** judges text with reader tests and keeps the meaning fixed.

If you want more than one, each runs as its own loop with its own rubric. Blending them makes the scores meaningless.

### 2. Goal and what's off limits

What should be better, in your words. Which screens or texts are yours to handle. How much change you'll accept: tighten, restyle or redesign (writing: line edit, rewrite or restructure).

**Effect:** becomes the IN and OUT scope lists in every brief. Judges ignore OUT-of-scope parts, even ones that look wrong, and implementers are told not to touch them. Change tolerance stops implementers from over-reaching. Most people want tightening, and the briefs say so.

### 3. Focus

Which dimensions matter most. Polish offers consistency and tokens, typography, spacing, legibility, hit targets, motion and performance. Art direction offers the traits of the look. Writing offers clarity, concision, structure, voice, consistency, usefulness and accuracy.

**Effect:** sets the rubric weights, and so what the judges put first. It also decides which audits run in round 0. For example, if consistency is in focus, a token audit runs before any fixes.

### 4. Baseline

Your own score out of 10 for where things are today, the judges' score, or both (recommended).

**Effect:** calibrates the judges. If you say 6 and they say 8, the rubric is too soft, and it's tightened before round 1 so later scores mean something.

### 5. Stop condition

A target score (e.g. ≥ 9 in every group and theme), a number of rounds, or whichever comes first (recommended).

**Effect:** decides when the loop ends. Even without a cap, it stops early and shows you the remaining fix list if two rounds in a row improve by less than about 0.2. What's left at that point is usually a judgment call that's yours to make.

### 6. Subagent intelligence

Best available, balanced (your session's model) or fast (a cheaper model for capture and mechanical fixes, a strong one for judging).

**Effect:** sets the model and effort for each role. Judging is where intelligence pays most, so savings come from capture and implementation first.

### 7. References

Sites, apps, screenshots, a Figma file, a design system, a style guide, or voice samples for writing. It also asks what you like about each one.

**Effect:** they're saved to `.designloop/references/`, so every round judges against the same thing. Your "what I like" answers become concrete, checkable traits, like "row height about 28px" or "one accent colour". Without the why, a reference gets copied wholesale.

Each reference is labelled by authority:
- **Spec:** a Figma file to match. If the match is exact, any difference is a defect.
- **Reference:** match the named traits only.
- **Generated target image:** for feel only. Implementers never add controls because a picture shows one.

If you have nothing for art direction, it offers to gather examples or generate target images from your real screenshots. It tells you first if that costs credits.

### 8. Delivery

A branch, an unmerged PR and/or a build you can run.

**Effect:** the final step does exactly that, and nothing gets pushed or merged beyond it.

### Writing mode also asks

- **Who the reader is and what they should do after reading.** This drives the reader test: a fresh agent reads each text cold, and anywhere its understanding differs from your intent goes to the top of the fix list.
- **Voice samples.** Writers match these instead of a generic house style.
- **What's fixed.** Facts, legal text and names are never reworded. Problems with them are flagged to you instead.
- **Length and translation limits.** Strings are checked in place for truncation, and stale translations are listed.

### Then it studies your project and asks only what it can't infer

It reads CLAUDE.md, AGENTS.md, README and CONTRIBUTING, the package scripts, the checks the project trusts, your git habits, what the app could reach outside the repo (accounts, paid APIs, real data), and whether a Figma connector or image generation is available. Then it confirms only the open choices, stating what it found ("I found X, so I plan to Y"):

- **Where to work:** current branch, new branch, or a worktree per implementer. Parallel implementers need worktrees.
- **How to test:** dev server only, or also the production build; which themes, sizes and devices count.
- **What must not be touched or spent:** real accounts and paid services, and whether a mock mode is fine for captures.
- **How much runs at once:** parallel implementers, or one at a time.

These become `.designloop/RULES.md`, with each rule marked as coming from you, from your docs, or inferred.

## The loop

1. **Round 0:** build a one-command capture harness (stills per theme and state, clips for motion; text snapshots in writing mode). Capture the untouched app, run any audits, and have fresh judges score each group. You get the baseline and the plan.
2. **Round 1 is the foundation:** tokens and shared components in polish mode, the core of the look in art direction, the glossary and voice in writing. It runs alone, so parallel implementers don't each invent their own.
3. **Each later round:** implementers fix their share of the fix list, the work is merged and checked, everything is re-captured with the same harness, and *new* judges re-score. They see the previous verdict so the scale stays steady and regressions get named. Regressions are fixed before new items.
4. **Finish:** final checks and capture, your chosen delivery, and a report. The report has scores round by round (e.g. `5.0 → 6.75 → 8.5 → 9.15`), before/after images, what was left alone on purpose, and what's still on the list.

## What's inside

```
skills/designloop/
  SKILL.md                         # the loop
  references/
    mode-polish.md                 # polish rubric and questions
    mode-art-direction.md          # turning a look into scoreable traits
    mode-writing.md                # copy and prose: rubric, reader tests, guard rails
    rubric.md                      # shared rubric header + judge output format
    briefs.md                      # RULES.md checklist, judge/auditor/implementer briefs
```

Works best with something that can drive your app (Playwright/CDP for web and Electron, simulator tooling for mobile). Optional: a Figma MCP connector for matching designs, and an image-generation tool for target images.

## Inspiration

- **[dream-loop](https://github.com/achimala/dream-loop)** by [@anshuc](https://x.com/anshuc): a skill that dreams up a target screenshot with image generation, builds toward it, and has a separate critic subagent compare the two until it's satisfied. designloop takes its judging method (a fresh, nitpicky judge every round, with regressions scoring lower) and uses it to polish an existing UI.
- **[Agents Sucked At UI (Until Today)](https://www.youtube.com/watch?v=xq0eB8vxeqk)** by Theo ([t3.gg](https://t3.gg)): Theo hill-climbs the T3 Code interface by having agents revise the UI, screenshot it and get it scored by several models each round. That video is where the "capture → score → fix → repeat" shape for UI design comes from.

## License

MIT
