# Rules and brief templates

Subagents know only what the brief and the files it points to tell them. Keep briefs short and point at BRIEF.md, RUBRIC.md and RULES.md instead of repeating them.

## What RULES.md should cover

RULES.md records what step 2 found about this project and what the user chose. Don't copy rules from another project: every line should come from the repo, the machine or the user. Mark each as "from the user", "from the project's docs" or "inferred", so a subagent knows how firm it is. Cover:

- **Repo and stack:** path, framework, and how to get a working checkout (branch or worktree, per the user's choice) without disturbing theirs.
- **Running and driving the app:** the commands, and the tool used for captures. If several agents run at once, how each keeps its own instance (profile, ports) separate.
- **Outside effects:** what the app can reach (accounts, paid services, messaging, real data, the user's own devices) and what the user decided: mock, sandbox, or allowed. How to confirm a mock is in effect.
- **Processes and resources:** stop only what you started; any limits found (disk, ports, other running agents) and how to clean up temporary files.
- **Checks:** the project's own type check, lint, test and build commands, and their state before the loop began.
- **How to test:** dev only or also the production build; which themes, sizes or devices.
- **Design bar:** reuse existing tokens and components; new things must look like they belong; cover every theme; respect reduced motion.
- **Evidence:** where before/after stills and clips go.
- **Git:** where to commit, the project's commit conventions, and that nothing is pushed, merged or released unless the user asked.
- **Report:** what changed (files), evidence paths, what was not done or not verified, and anything worth logging as a future issue.

## Judge brief

```
You are an independent design judge in a design loop for <app> (mode: <polish | art direction>). You did not build any of this.
Read <RUBRIC.md> (scope, rubric, output format) and the references it lists.
Judge only the IN-scope surfaces in group <X>: <list>. Ignore OUT-of-scope surfaces even if they look wrong.
Captures: <round-N/> (index.json maps each file to surface, state and theme). Look at every still for
your group in every theme, and step through the clips frame by frame for anything that moves.
Measure rather than guess: sample colours, measure px, count frames.
<If references exist:> The user's references are in <references/> (README.md says what they like about
each). Say where the UI falls short of those traits. A Figma spec marked exact is authoritative: list every measurable difference from its frames.
A generated target image is a direction for feel and finish only; ignore details it invented, and don't ask for controls or behaviour it shows.
<If N > 0:> Previous verdict: <round-(N-1)/judge-X.md>. List regressions explicitly, keep the scale
consistent with it, and don't raise a score unless the punch list shrank to justify it.
Be nitpicky and honest; the user rated today's UI about <baseline>/10 and wants <target>.
Write your verdict to <round-N/judge-X.md> in the rubric's output format. Do not edit any source code.
```

## Token auditor brief (round 0, when consistency is in focus)

```
Read-only audit of <app>'s styling for a polish loop. Inventory every colour, radius, type size and
weight, spacing step, shadow, border, z-index, duration and easing actually used in the IN-scope
surfaces (<RUBRIC.md>), per theme. Report: (1) the inventory with counts and where each is used,
(2) a proposed canonical token table per theme, (3) a migration map from current values to tokens,
(4) risky spots — shared classes or variables that also style OUT-of-scope things, which implementers
must not change by accident. Write to <round-0/token-audit.md>. Change no code.
```

## Implementer brief

```
You are an implementer in a design polish loop for <app>. Goal: <goal>, as a tightening pass — the
user likes the current look; do not redesign.
Read first: <BRIEF.md>, <RUBRIC.md>, <RULES.md>, <token-audit.md> (note the risky spots), and
<round-N/judge-X.md>. Your items: <P0 #a–#b, P1 #c…>, covering <surfaces>.
<If references exist:> <references/README.md> lists the traits the user wants; target images show the
intended feel. Match the feel within the scope; don't add controls or change behaviour because a
picture shows it.
Use the existing foundation (<tokens file, shared material/motion styles>); add no ad-hoc values.
Do not touch: <OUT list, and files another implementer owns this round>.
Work on branch <name> in <worktree>. Verify in the running app in every theme, capture before/after
evidence to <path>, run the project's checks, commit. Don't push or merge.
Report: what you changed, evidence paths, items not done or not verified, future issues.
```

The foundation implementer (round 1) gets the same brief with a different scope: define the canonical tokens once per theme, build the one shared menu/popover material and motion spec, and apply them to the surfaces that share them. Everyone after builds on that.

## Writing mode briefs

The judge brief above works with "texts" for "surfaces" and "snapshots" for "captures"; add: "Quote the current text and give a proposed rewrite for every item. Read the reader-test reports in <round-N/reader-*.md> and treat each mismatch with the purpose in BRIEF.md as P0. Check every claim against <code / product / sources> and list what you couldn't verify." The terminology and voice auditor and the writers get these:

```
Reader test for <app / piece>. You are a <audience from BRIEF.md>. You have not seen this before.
Read <round-N/texts/X> and, if present, its screenshot in place. Don't look anything else up.
Report: (1) in one or two sentences, what it says; (2) what you would do next; (3) every place you
hesitated, re-read, or weren't sure what a word or instruction meant; (4) anything that sounded off.
Write to <round-N/reader-X.md>. Don't suggest rewrites.
```

```
Read-only terminology and voice audit for <app / piece> (writing loop). Across the IN-scope texts in
<RUBRIC.md>, inventory: the terms used for each concept and where; capitalisation, punctuation, number
and date patterns; recurring message shapes (errors, confirmations, buttons); tone by surface; breaks
from <style guide>. Report: (1) the inventory with counts, (2) a proposed glossary, one term per
concept, (3) a short voice sheet with examples from the best existing texts, (4) risky spots —
strings reused in several places, matched in code/tests/analytics, translated, or legal.
Write to <round-0/voice-audit.md>. Change nothing.
```

```
You are a writer in a writing loop for <app / piece>. Goal: <goal>, as a <line edit | rewrite |
restructure>. Reader: <audience>; purpose: <what they should know or do>.
Read first: <BRIEF.md>, <RUBRIC.md>, <RULES.md>, <voice-audit.md> (glossary, voice sheet, risky
spots), the voice samples in <references/>, and <round-N/judge-X.md>. Your items: <…>, covering <texts>.
Keep the meaning: no new claims, features, numbers or caveats; flag accuracy problems instead.
Keep keys, placeholders, plural forms and markup exactly; edit source-language strings only.
Do not touch: <OUT list, fixed/legal text, texts another writer owns this round>.
Work on branch <name>. Run the project's checks and i18n tooling, re-render anything with a length
limit and check it in place, commit. Don't push or merge.
Report: each change as before → after with a one-line reason, items not done, claims you couldn't
verify, translations now stale.
```
