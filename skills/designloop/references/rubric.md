# Rubric header and judge output format

Every RUBRIC.md has this header, the parts from the mode's file, and this output format.

## Header (fill in)

- **Mode:** professional polish, art direction or writing.
- **Goal:** from ~X/10 to ≥ Y/10, as a <tightening pass | restyle | redesign>. What the user likes and wants kept.
- **IN scope:** every surface (or, in writing mode, every text), by name.
- **OUT of scope:** surfaces the user owns. Do not critique or change them.
- **Themes / platforms:** e.g. light AND dark. In writing mode: the reader, the purpose, and the contexts (states, locales, length limits, medium).
- **References:** design guidelines and skills every judge and implementer should read, with paths; and, if the user gave or chose any, the designs they like (`references/README.md`) with the specific traits to aim for.
- **Spec (if any):** a Figma design the user said to match, in `references/figma-*.png` and `figma-spec.md`, and whether the match is exact or guided.
- **Generated target images (optional):** `references/target-*.png`. A direction for overall feel, hierarchy and finish, not a spec: score with the parts below, and never mark the UI down for lacking something only the target shows.

## Parts

Take the parts and default weights from the mode's file: `mode-polish.md`, `mode-art-direction.md` or `mode-writing.md`. Keep the total at 10. Score each surface and overall; fractions are allowed. Be nitpicky and honest, and define the top of the scale (e.g. "9 = I'd ship this in a polished commercial app.").

## Judge output format

1. **Per-surface table:** surface · score per theme · one-line verdict.
2. **Overall score:** each rubric part and the total, per theme. Show the history when earlier rounds exist (`5.0 → 6.75 → 8.5`).
3. **Punch list, prioritised** (P0 = most visible or most inconsistent). Each item: surface, what is wrong, what right looks like in concrete terms (token, px, ms, easing, contrast ratio; in writing mode, the current text quoted and a proposed rewrite with its reason), and which theme or context. Cross-cutting issues are their own items.
4. **Regressions** since the previous round, listed explicitly. Keep scoring consistent with the prior verdict; if something got worse, score it lower.
5. **How it was checked:** which stills and clips were examined, and anything that could not be judged from the captures.
