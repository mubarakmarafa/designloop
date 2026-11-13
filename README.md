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

The skill interviews you first (goal, what's off limits, focus, baseline score, stop condition, model budget, references, delivery), studies how your project runs and ships, then builds a repeatable capture harness and starts the loop. By default it delivers a branch and an **unmerged** PR — it never merges or releases unless you ask.

**Cost note:** each round spawns several subagents. Set a round cap when it asks for a stop condition.

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
