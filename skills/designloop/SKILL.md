---
name: designloop
description: Run a design loop — an iterative pass where the app's UI is captured (screenshots and clips), scored by independent judge subagents against a rubric, fixed by implementer subagents, then re-captured and re-judged until a target score or round limit is hit. Two modes: professional polish (consistency, tokens, legibility, spacing, hit targets, light/dark parity, motion, performance) and art direction (moving the UI toward a specific look and feel, a reference, a generated target image or a Figma design). Use this whenever the user says "designloop" or "design loop", asks for a serious visual/UI polish pass, wants the UI taken "from a 6 to a 9", wants subagents to judge or critique a UI and iterate, wants the UI restyled toward an aesthetic or made to match a Figma file or reference, or asks for a dream-loop-style screenshot → judge → fix cycle on an existing interface. Not for building a new UI from scratch or for a single small visual fix.
---

# designloop

A loop that raises the quality of an existing interface by measuring it, not by guessing. Each round: capture the UI the same way, have fresh judges score it against a fixed rubric, fix what they found, repeat. It is modelled on [dream-loop](https://github.com/achimala/dream-loop)'s judging method (a fresh judge every round, nitpicky, regressions score lower), applied to UI polish instead of matching a target image, and on Theo's (t3.gg) hill-climbing approach to UI design in [Agents Sucked At UI (Until Today)](https://www.youtube.com/watch?v=xq0eB8vxeqk).

The loop works because of three separations. Keep them:

- **Judges never implement and implementers never judge.** An agent that wrote the fix scores it generously.
- **Judges are fresh each round.** A judge who saw the last round anchors on its own earlier verdict. Give a new judge the rubric, the new captures and the previous verdict (so scoring stays consistent and regressions are named), not the conversation.
- **The capture set is fixed.** Scores are only comparable if round N shows the same surfaces, states and themes as round 0.

## Two modes

The loop is the same in both; what the judges measure against is different. Settle the mode first, because it decides the rubric, the questions worth asking and how much change is expected.

- **Professional polish** — an objective pass, the things a senior product designer checks: one set of tokens and surfaces used everywhere, fewer type sizes and weights, a consistent spacing scale, legibility and contrast, hit targets and cursor zones, state coverage, light/dark parity, motion quality. The target is the app's own best self; taste changes are out. Read `references/mode-polish.md`.
- **Art direction** — a subjective pass toward a specific look and feel: a named style ("graphic realism", "neon pop with ASCII art and brutalist, dynamic layouts"), a set of references, a generated target image, or a Figma design to match. Judges score how well the UI expresses that direction. Bigger visual change is expected, so fonts, colour and layout character are in play. Read `references/mode-art-direction.md`.

Infer the mode from the request when it is clear ("tighten", "consistent", "sloppy" → polish; "make it look like", "more artistic", a style name, a Figma link → art direction), otherwise ask. A user can want both: run art direction first to set the look, then a polish pass to make it consistent, as two loops with their own rubrics. Don't blend them into one rubric, or judges trade consistency against style and the scores stop meaning anything.

## Step 1: Interview the user

Settle the mode, then ask the rest with AskUserQuestion (two calls; it takes up to four questions each). Skip any question the user's message already answered, and offer a recommended option first. The answers go in the brief file (step 3), which is what every subagent reads.

1. **Goal** — what should be better when this is done, in their words? ("Chrome feels inconsistent", "menus are sloppy", "feels janky".) Also ask what is **off limits**: surfaces they want to handle themselves, and how much change they will accept (tighten vs. restyle vs. redesign). Most users want tightening, not a redesign; say so in every brief or implementers will over-reach.
2. **Focus** (multi-select) — which dimensions matter most; this sets the rubric weights. The options depend on the mode and are listed in its reference file (polish: consistency and tokens, typography, spacing and layout, legibility, interaction and hit targets, motion, performance; art direction: the traits of the chosen look, and which parts of the UI should carry it).
3. **Baseline** — where is the UI today? Either the user gives a score out of 10, or the judges score it first. Recommend doing both when the user has an opinion: their number calibrates the judges (if they say 6 and judges say 8, the rubric is too soft — tighten it before round 1).
4. **Stop condition** — a target score (e.g. ≥9/10 in every group and theme), a number of rounds, or whichever comes first. Recommend "target score, capped at N rounds". Warn that an uncapped loop can use a lot of tokens.
5. **Subagent intelligence** — which model and effort judges and implementers run at. Offer: best available (slowest, most thorough), balanced (the session's own model and effort), fast (cheaper model for capture and mechanical fixes, strong model for judging). Judging is where intelligence pays most; if the user wants to save, save on capture and mechanical implementation first.
6. **References** — do they have designs they like or must match? Websites, apps, screenshots, a Figma link, a design system, or a named guideline or skill (a platform HIG, a design-engineering skill). Ask what they like about each (the density, the menus, the motion), because "like this site" without a reason gets copied wholesale. In polish mode references are optional and usually guidelines. In art direction mode the loop needs a target: a style description, references, a Figma design or a generated image; if the user has none, offer to gather or generate some (see "References and targets").
7. **Delivery** — a branch only, a PR (not merged), and/or a build they can run. Default: branch + unmerged PR. Never merge or release unless they ask.

Put everything the user gave you in `.designloop/references/` (step 3) and list it in the rubric, so every judge and implementer works from the same sources.

## Step 2: Study the project's setup

Before writing any rules, find out how this project is actually run, tested and shipped, so the loop follows the user's habits instead of imposing its own. Look first, ask second: read what the repo and the machine can tell you, and only ask about what is still unclear or is a matter of preference.

Look for:

- **Instructions already written down:** CLAUDE.md / AGENTS.md, README and CONTRIBUTING, memory notes, and any earlier agent rules files. These win over anything inferred.
- **How the app runs and how it can be driven:** package scripts, Makefile, dev vs. production builds, and what can capture it (browser/CDP, Playwright, simulator or device tooling, an existing screenshot or e2e setup). If nothing can drive the app automatically, say so and ask how the user wants captures made.
- **The checks the project trusts:** type check, lint, tests, build, and whether they pass today.
- **Git habits:** default branch, branch naming, whether worktrees are already in use, commit message conventions, whether work goes through PRs.
- **Anything a run could touch outside the repo:** real accounts, paid APIs or AI backends, messaging, production data, the user's own app profile or devices. Find out whether there is a mock, demo or sandbox mode.
- **Whether a design tool is connected:** if the user gave a Figma link (or other design-tool link), look for a connector or MCP server that can read it, and whether it is signed in.
- **Whether images can be generated here:** check what this session can actually call, rather than assuming. Some harnesses have image generation built in (Codex does); in others it comes from an installed skill, plugin or MCP server (Higgsfield or any other image model). Look through the available tools and skills for one. If there is none, the loop still works; target images are optional.
- **Machine limits that matter for parallel work:** free disk space, ports already in use, other agents or dev servers running.

Then confirm with the user, in one AskUserQuestion call, only the choices you could not settle from what you found. Typical ones:

- **Where to work:** the current branch, a new branch, or separate worktrees per implementer (needed if implementers run in parallel).
- **How to test:** dev server only, or also the production/packaged build before finishing; which themes, window sizes or devices count.
- **What must not be touched or spent:** real accounts and paid services, and whether a mock mode is acceptable for captures.
- **How much to run at once:** parallel implementers, or one at a time.

State what you inferred in the question ("I found X, so I plan to Y") so the user can correct it quickly. Write the result to RULES.md (next step), and note which rules came from the user and which you inferred.

## Step 3: Set up

Create a working folder outside the source tree, or a git-ignored `.designloop/` in the repo:

```
.designloop/
  BRIEF.md        # the interview answers: goal, scope in/out, change tolerance, focus, stop condition
  RUBRIC.md       # from references/rubric.md, weighted for the chosen focus
  RULES.md        # this project's working rules, from step 2 (see references/briefs.md)
  references/     # what the user likes, and any target images (see below)
  capture/        # the capture script(s) and the surface list
  round-0/ …      # per round: stills per theme, videos/, index.json, judge-*.md
```

Write these before spawning anything. Subagents start with no context, so the files are the context: point briefs at them instead of retyping.

- **RUBRIC.md** — start from the rubric in the mode's reference file (`references/mode-polish.md` or `references/mode-art-direction.md`), with the shared header and judge output format from `references/rubric.md`. Adjust the weights to the user's focus, write the IN and OUT scope lists explicitly, and define what the top score means.
- **RULES.md** — what step 2 established: how to run and drive the app, where to work in git, the checks to run, what must not be touched or spent, and how to clean up. `references/briefs.md` has the list of things it should cover.
- **Surface list** — every surface in scope, each with the states worth judging (rest, hover, open, selected, empty, error, busy) and each theme. Split it into two or three groups of related surfaces so each judge can look closely; one judge across fifty screenshots skims.

Check that the project's checks pass before you start, so a later failure is yours.

## References and targets

A rubric says what good is in general; references say what good looks like *to this user*. Art direction mode always has a target of some kind. Polish mode can run on the rubric alone, with the app's own best surfaces as the standard.

There are three kinds of target, and they carry different authority. Say which one each file is in `references/README.md`, because judges and implementers must treat them differently:

- **A spec** (a Figma design the user says to match): authoritative. Match it.
- **A reference** (sites, apps, screenshots the user likes): match the named traits, not the whole thing.
- **A generated image**: a direction for feel only.

**References the user gives.** Save them to `.designloop/references/` with a short `README.md`: for each one, what it is and what the user likes about it. For a website or app, capture screenshots of the relevant parts (and a clip if the appeal is motion) so judges look at the same thing every round instead of re-fetching a page that may change. Turn the likes into concrete, checkable traits in the rubric ("row height about 28px", "one accent colour", "menus fade in under 200ms"), since a judge can score a trait but not a vibe.

**References you gather.** If the user wants a direction but has no examples, propose three or four well-known products whose interface fits their goal, show them, and let the user pick. Don't choose for them.

**A Figma design.** If the user gives a Figma link, check whether a Figma connector or MCP server is available in this session (step 2). If it is, load its own skill or instructions first, then pull for each linked frame: a rendered image, and the design data behind it (variables and styles, type, spacing and auto-layout values, colours, radii, effects, component names, any motion). Save the images as `references/figma-<surface>.png` and the values as `references/figma-spec.md`, mapped to the app's surfaces and states, and note anything the file doesn't cover (states, themes, sizes). If no Figma tool is available, ask the user to connect one or export the frames as images plus the values they care about; never ask them to paste a token.

Ask how literal the match should be: exact (the Figma is the spec; any difference is a defect) or guided (follow its system and feel, keep the app's own structure where they differ). With an exact match the loop becomes convergence: each round, judges compare each capture to its frame at the same size, list every measurable difference (px, colour, weight, radius, spacing, missing or extra elements), and score by how many remain and how visible they are. Where the design and the running app disagree about content or behaviour that the user didn't ask to change, flag it rather than silently following either.

**Target images you generate.** If image generation is available (step 2) and the user wants it, make a "polished version" of their own UI to aim at:

1. Start from real screenshots of the app (the round 0 captures, or ones the user provides), one per key surface. Editing from the real screenshot keeps the layout, content and identity; generating from a text description alone drifts into a different product.
2. Prompt for the same screen, professionally polished: same layout and content, refined spacing, type, colour and hierarchy, consistent with the user's goal, focus and references. Say it is a realistic screenshot, not concept art, and name what must not change (the OUT-of-scope surfaces, the brand, the structure).
3. Make two or three options that differ in a way the user can choose between (for example quieter and denser, or softer and more spacious), and show them side by side with the original. Ask which one, or which parts of each, they want. If none fit, ask what's off and regenerate, or drop the idea.
4. Save the chosen image(s) as `references/target-<surface>.png`, and write down in the README what the user liked about it.

Treat a generated target as a direction, not a spec. Image models invent details: illegible text, impossible layouts, controls that don't exist, a look that ignores the platform. Judges compare the real UI to the target for overall feel, hierarchy and finish, and still score with the rubric; an implementer should never add a control or change behaviour because the picture showed one. If the target and the user's scope disagree, the scope wins.

Tell the user before generating if it costs them credits, and how many images you plan to make.

## Step 4: Round 0 — capture and baseline

1. **Build the capture harness first.** A script that drives the real app to every surface and state and writes stills (and short clips for anything that moves) plus an `index.json` mapping file → surface/state/theme. Use whatever the project supports (CDP/Playwright for Electron and web, simulator tools for mobile). It must be repeatable with one command, because it runs every round. Stills at 2× where possible; clips at ≥30fps for motion.
2. **Capture round 0** on the untouched code.
3. **Audit (polish mode, or whenever consistency is in focus).** One read-only agent inventories every colour, radius, type size, shadow, spacing and duration actually used, proposes a canonical token table, a migration map, and the *risky spots* (shared classes that also style out-of-scope things). Implementers need this before round 1.
4. **Judge.** One fresh judge per surface group, in parallel, each given RUBRIC.md, the capture folder, the references folder if there is one, and the judge brief from `references/briefs.md`. Each writes `round-0/judge-<group>.md`: a per-surface score table, the overall score by rubric part and theme, and a prioritised punch list where every item says what is wrong and what right looks like in concrete terms (token, px, ms, easing).
5. **Calibrate.** Compare the judges' baseline to the user's number. If they disagree by more than about a point, tell the user and adjust the rubric before continuing.

Tell the user the baseline and the plan for round 1 in a few lines, then keep going; don't wait for approval unless they asked to review each round.

## Step 5: The loop

Each round:

1. **Plan** from the punch lists. Do cross-cutting foundations first (tokens, the shared menu/popover material, motion constants), alone, in round 1. Later work builds on them, and parallel implementers who each invent tokens produce exactly the inconsistency you are fixing. After the foundation, split the remaining items by surface group and run implementers in parallel if RULES.md allows it (each needs its own checkout), otherwise one after another.
2. **Implement.** Brief each implementer with: BRIEF.md, RUBRIC.md, RULES.md, the references, the judge file and item numbers they own, the foundation they must reuse, the risky spots, and what they must not touch. Ask for before/after evidence and an honest list of what was not done or not verified. Fix small leftover findings yourself instead of spawning an agent for them.
3. **Integrate.** Merge the round's branches, resolve overlaps (two agents editing one shared file is common), run the project's checks on the combined code.
4. **Re-capture** with the same harness into `round-N/`. Add surfaces only when a fix created a new state; never drop one.
5. **Re-judge** with fresh judges, giving them the previous round's verdict so they list regressions and keep the scale steady.
6. **Decide.** Stop if the stop condition is met. Otherwise go again on the remaining items.

Things that go wrong, and what to do:

- **Regressions.** A round that raises one surface and breaks another is normal. Judges must list regressions; fix those before new items.
- **Plateau.** Gains shrink as scores rise (expect big jumps early, then +0.1–0.4 per round near the top). If two rounds move less than ~0.2 and the target isn't met, stop and show the user the remaining punch list rather than grinding; what's left is usually a judgment call that is theirs.
- **Score inflation.** If a judge jumps a score without the punch list shrinking to match, distrust it. Re-judge that group with a new judge told only to find faults.
- **Scope creep.** Judges will critique out-of-scope surfaces and implementers will "improve" them. Both briefs restate the OUT list.
- **Motion and performance can't be judged from stills.** Use clips, read frame by frame, and for performance measure (frame times during the interaction, idle CPU) instead of eyeballing.
- **Dev and production can differ.** Stylesheet order, minification and platform features sometimes change between a dev server and a shipped build. If the user chose to test the production build, capture the final round from it.
- **Side effects.** Follow RULES.md for anything outside the repo. If a run turns out to need something the rules don't cover (a real account, a paid call, a shared device), stop and ask rather than guess.

## Step 6: Finish

1. Run the project's checks once more, and capture a final set the way the user chose to test (step 2).
2. Deliver what was agreed: push the branch, open the PR without merging, and/or produce the build with instructions to run it.
3. Report to the user, briefly:
   - the score per group and theme, round by round (a small table, e.g. `5.0 → 6.75 → 8.5 → 9.15`);
   - what changed, by surface, in plain language;
   - before/after images for the most visible changes;
   - what was left alone on purpose, anything not verified, and the remaining punch-list items with a recommendation;
   - where the evidence lives.

Say plainly if the target was not reached and why.

## Reference files

- `references/mode-polish.md` — professional polish: what to ask, the rubric, what judges measure. Read when the mode is polish.
- `references/mode-art-direction.md` — art direction: turning a look into scoreable traits, the rubric, guard rails. Read when the mode is art direction.
- `references/rubric.md` — the shared rubric header and the judge output format. Read when writing RUBRIC.md.
- `references/briefs.md` — what RULES.md covers, and the judge, auditor and implementer brief templates. Read before spawning the first subagent.
