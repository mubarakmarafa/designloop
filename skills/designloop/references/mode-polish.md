# Mode: professional polish

An objective pass. The question each round is "would a senior product designer sign this off?", not "do I like it?". The look stays; the execution gets tighter. If a fix would change the app's character (a new typeface, a new palette, a different layout idea), it belongs in an art direction loop, so raise it with the user instead of doing it.

## Focus options to offer

- **Consistency and tokens** — one set of colour, radius, shadow, border and surface values; the same component looks the same everywhere; themes are true counterparts.
- **Typography** — fewer sizes and weights on a clear scale, consistent line heights, one treatment per role (title, label, body, meta), no near-duplicates (13 and 13.5px).
- **Spacing and layout** — a spacing scale that is actually used, aligned edges and baselines, consistent padding inside like components, sensible density.
- **Legibility** — text and icon contrast, minimum sizes, truncation, readable states (disabled, selected, error) in every theme.
- **Interaction and hit targets** — target sizes and the gaps between them, the cursor shown over each zone, hover/press/focus/selected states present and matching, keyboard focus order, click-away and Esc.
- **Motion** — purposeful, fast, correctly eased, interruptible, reduced-motion respected.
- **Performance** — frame time during the interactions that matter, zero work at idle.

## Rubric (default weights; total 10)

- **Consistency & tokens (0–3)** — one set of colour, type, radius, spacing, shadow and border values used everywhere; menus, popovers, panels and chips look like one family; themes are true counterparts (same hierarchy, contrast and states, not ad-hoc overrides); no one-off values.
- **Hierarchy & craft (0–3)** — type scale and number of styles, spacing rhythm, alignment, icon size and weight, density, legibility and contrast, clear primary and secondary actions, nothing loud that shouldn't be, considered empty states; feels native to its platform.
- **Interaction & motion (0–3)** — hit targets and cursor zones are generous and unambiguous; press, hover and focus states exist and match; every animation has a purpose, is fast (about 150–250ms) and eased (ease-out to enter, never linear); popovers grow from their trigger; no layout jank or flashes; click-away and Esc behave; reduced motion respected.
- **Details (0–1)** — hairlines, optical alignment, truncation, focus rings, icon baselines, shadow crispness, consistent corner radii.
- **Performance (when in focus; take the points from the parts the user cares about least)** — measured frame time and idle cost.

Top of the scale: "9 = I'd ship this in a polished commercial app."

## What judges should measure

Counting beats impressions in this mode, and counts make progress visible between rounds:

- distinct font sizes, weights and line heights in scope, and which are near-duplicates;
- distinct colours, radii, shadows and spacing values, against the token table;
- contrast ratios for text and essential icons, per theme;
- hit target sizes and the cursor over each interactive zone;
- durations and easings, read from clips frame by frame;
- which states each control has (rest, hover, press, focus, selected, disabled) per theme.

The token audit in round 0 gives implementers the map. Do the shared foundation first.
