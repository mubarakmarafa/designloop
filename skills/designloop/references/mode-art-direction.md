# Mode: art direction

A subjective pass toward a specific look and feel. The work is to make a matter of taste scoreable, so that judges agree with each other and with the user, and rounds actually converge.

## Pin down the direction first

A style name means different things to different people. Before round 0, turn the direction into a short written **direction brief** in `references/README.md`, and get the user to agree to it:

1. **The direction in a sentence**, in the user's words.
2. **Five to eight traits a judge can look for.** For example, "neon pop with ASCII art and brutalist, dynamic layouts" might become: a near-black ground with two or three saturated neon accents; monospaced type for labels and data; ASCII or box-drawing ornament for dividers and empty states; hard edges, visible grid lines and no soft shadows; asymmetric, oversized headings; motion that snaps rather than eases. Each trait should be something you could point to in a screenshot.
3. **Anti-traits:** what this look is *not* (for example: no gradients, no rounded pills, no pastel).
4. **Where it applies and how strongly:** which surfaces carry the look loudly (landing, headers, empty states) and which stay quiet for usability (dense tables, forms, long reading). A look applied at full strength everywhere usually hurts use.
5. **What stays fixed:** brand elements, layout structure, anything OUT of scope.

Sources for the traits: the user's references and what they like about each; a Figma design; a chosen generated target image; or, when the user only has a name for the style, a few examples you gather and show them. If you are unsure what the user means by a term, ask or show options; don't guess and build on it.

## Focus options to offer

- which traits matter most (often two or three define the look);
- which surfaces should carry it;
- how far to go: an accent on the existing design, a restyle, or a full redesign of the visual layer;
- whether type, colour, layout, imagery/ornament and motion are each in play.

## Rubric (default weights; total 10)

- **Fidelity to the direction (0–4)** — the traits in the direction brief are present and unmistakable, the anti-traits are absent, and it reads as that look at a glance, not as the old UI with a new accent colour. With a Figma spec set to exact match, this part is the measured difference from the frames.
- **Cohesion (0–2)** — the look is applied as one system: the same type, colour, ornament and motion rules across every in-scope surface and theme, with deliberate loud and quiet zones rather than patchy coverage.
- **Usability held (0–2)** — still legible and operable: text contrast, hit targets, clear states and hierarchy, nothing important hidden by ornament. A striking screen that is harder to use scores low here; this part is the guard rail.
- **Craft and motion (0–2)** — execution quality within the style: alignment, spacing, type setting, asset quality, and motion that matches the look.

Top of the scale: "9 = a designer known for this style would recognise it as a strong, finished example, and it is still pleasant to use."

## How this mode differs in the loop

- **Round 0 judging** scores the current UI against the direction, so the baseline is usually low. That is expected; tell the user.
- **Round 1 is the foundation of the look**: the type choices, the palette, the core surface and ornament rules, applied to one or two representative surfaces. Show the user this before spreading it everywhere, because a wrong direction is cheap to change at this point and expensive later. This is the one place the loop should pause for the user by default.
- **Later rounds** spread the look across the remaining surfaces and raise cohesion and craft.
- **Fonts and assets:** check licences and how the project loads fonts before adding any; prefer what the user already has or names. Say what was added.
- **Judges** get the direction brief and the references every round, and say which traits are missing or weak on each surface, not just a score.
- **After it lands**, offer a polish-mode loop to tighten consistency within the new look.
