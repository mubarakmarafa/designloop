# Mode: writing

The same loop, applied to words instead of pixels: interface copy (buttons, labels, errors, empty states, onboarding, notifications), product and docs pages (README, help articles, landing pages, release notes), or standalone pieces (a post, an essay, a proposal, an email). Each round: snapshot the texts the same way, have fresh judges score them against a fixed rubric, revise, repeat.

The three separations matter even more here, because a writer who rereads their own draft hears what they meant, not what they wrote. Keep judges and writers apart, and keep judges fresh.

## What changes from the UI loop

Follow SKILL.md, with these substitutions:

- **Surfaces → texts.** The surface list is a list of texts in scope: files, sections, or UI strings grouped by flow (onboarding, settings, errors). Split them into two or three groups so each judge reads closely.
- **States and themes → contexts.** For UI copy, a string's context is where it appears and in what state (first run, error, empty, success), plus each locale or length constraint. For a page or piece, it is the audience and the medium (rendered page, email client, mobile width).
- **Capture → snapshot.** Each round, copy every in-scope text into `round-N/texts/` exactly as the reader gets it, with an `index.json` mapping file → text/section/context. Rendered matters: for UI copy, also capture a screenshot of each string in place (the UI capture harness works for this), because truncation, line breaks and what sits next to the words change how they read. For pages, capture the rendered page as well as the source. Never let a judge read only a string table; a label without its screen can't be judged.
- **Token audit → terminology and voice audit.** In round 0, one read-only agent inventories the terms used for each concept (is it "workspace", "project" or "space"?), capitalisation and punctuation patterns (title vs. sentence case, Oxford comma, ellipses, exclamation marks), recurring phrasings, tone shifts between surfaces, and any style guide's rules that are broken. It proposes a glossary (one term per concept) and a short voice sheet, and lists the *risky spots*: strings reused in several places, strings in code paths, API responses or legal text, and anything translated. Writers need this before round 1.
- **Foundation first → glossary and voice first.** Round 1 settles the glossary, the voice sheet and the patterns for recurring messages (error format, confirmation wording, button verbs) and applies them to one representative flow or section. Show the user this before spreading it: a voice they don't recognise as theirs is cheap to change now.
- **Implementers → writers.** Writers edit the texts where they live (source files, string catalogues, Markdown), keep keys, placeholders, markup and plural forms intact, and record each change as before → after with a one-line reason.

## Interview additions

Ask these alongside the step 1 questions (skip what the user already said):

- **Reader and purpose** — who reads this, what they already know, and what they should know, feel or do after reading. This is the most important answer; every rubric part depends on it.
- **Voice** — samples of writing the user considers on-voice (their own or others'), and what they like about each. A named style guide (an in-house guide, a platform's writing guidelines, AP or Chicago) goes in the references.
- **What's fixed** — facts, claims, names, legal and compliance text, quoted material, anything with approval behind it. Writers never change meaning here; judges flag problems instead.
- **Constraints** — length limits (characters for UI strings, words for pages), reading level, localisation (strings that get translated need room to grow and must avoid idioms), SEO terms that must stay.
- **Change tolerance** — line edit (tighten what's there), rewrite (same message, new words), or restructure (new order, sections cut or added).

## Focus options to offer

- **Clarity** — one idea per sentence, plain words, the point first, no ambiguity about what happens or what to do.
- **Concision** — no filler, hedges, throat-clearing or repetition; shortest form that keeps the meaning.
- **Structure and flow** — order that matches the reader's question, headings that scan, transitions, paragraph length.
- **Voice and tone** — sounds like one author throughout; tone fits the moment (calm in errors, brief in success, warm in onboarding).
- **Consistency** — one term per concept, one capitalisation and punctuation style, recurring messages follow one pattern.
- **Usefulness** — the reader can act on it: errors say what happened and what to do next, docs answer the real question, examples work.
- **Accuracy** — every claim, number, step and name is true of the product today.

## Rubric (default weights; total 10)

- **Clarity & usefulness (0–3)** — a reader in the stated audience understands it on first read and can do what it's for; the point comes first; errors and instructions say what happened and what to do; nothing ambiguous.
- **Concision & structure (0–2)** — every sentence earns its place; no filler, hedges, stacked qualifiers or repetition; order follows the reader's need; headings and lists scan; paragraphs and strings fit their space.
- **Voice & tone (0–2)** — sounds like one author with the user's voice; tone fits each moment; no corporate filler, hype, false cheer, or blame on the reader.
- **Consistency & mechanics (0–2)** — one term per concept matching the glossary; one capitalisation, punctuation, number and date style; recurring messages follow the same pattern; grammar and spelling clean; placeholders and markup intact.
- **Accuracy (0–1)** — every fact, step and name checks out against the product, code or source; nothing promised that isn't true.

Top of the scale: "9 = an experienced editor at a company known for its writing would publish this as is."

## What judges should measure

Writing invites impressions; count and test instead, so progress shows between rounds:

- **Reader test.** For each text, give a separate, fresh *reader* subagent only the text in its rendered context and the audience description, not the purpose. Ask it what the text says, what it would do next, and where it hesitated. The judge compares that to the intended purpose in BRIEF.md: every mismatch is a P0 clarity item. This is the most reliable check in the mode; don't skip it to save tokens.
- terms used for each concept, against the glossary, and every deviation;
- capitalisation, punctuation and pattern deviations from the voice sheet;
- filler words, hedges ("just", "simply", "basically", "might", "please note"), passive constructions that hide who acts, and sentences over about 25 words;
- strings over their length budget or truncated in the screenshots;
- claims checked against the code, product or sources, with each unverified claim listed;
- reading level, when the user set one.

Punch-list items quote the current text and give a proposed rewrite, with the reason ("Something went wrong" → "Couldn't save — you're offline. Your changes are kept and will sync when you reconnect." — says what happened, reassures, says what's next).

## Guard rails

- **Meaning is fixed unless the user said otherwise.** Writers make the same claims in better words; they don't add features, promises, numbers or caveats. If accuracy needs a claim changed, flag it for the user.
- **The user's voice, not the model's.** Judges mark down the tells of machine prose: stacked adjectives, "delve", "seamless", "leverage", "it's not X, it's Y", rule-of-three lists, em-dash habits, upbeat sign-offs. Writers match the voice samples, not a generic house style.
- **Strings are code.** Keep keys, interpolation placeholders, plural and gender forms, markup and escaping exactly; run the project's checks and i18n tooling after edits; don't touch strings that are matched in code, tests or analytics without checking for references.
- **Translated strings.** Edit the source language only, unless the user asks; note which translations are now stale.
- **Shorter isn't always better.** An error message that grows a sentence to say what to do next is an improvement. Judges score usefulness, not word count.
