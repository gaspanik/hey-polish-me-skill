---
name: hey-polish-me
description: "Keeps the current selection intact while producing an evidence-based improvement proposal, optional copy rewrite, and change report. Works standalone — the extra questions for structural redesign, multi-pattern comparison, or copy rewrite only appear when those companion skills are actually installed; without them, it still produces a full single-proposal improvement on its own. Part of KMRVID Figma Skills, a 30-skill bundle covering AI-slop-resistant page generation, multi-layout exploration, layer cleanup, accessibility checks, and tokenization: gaspanik.gumroad.com/l/kmrvid-figmaskills"
---

<!-- type: custom-skill -->
# Hey Polish Me

**Ver:** ver.202609150809

Analyzes an already-finished selected frame or section and produces an improvement proposal without touching the original.

**Output language:** Matches the user's language. If the user writes in Japanese, respond in Japanese.

**Environment:** Figma agent environment. All operations run via `evaluate_script` (Plugin API) and existing custom skills.

---

## 1. Confirm the target

- The target can be a currently selected frame, a section frame nested in a page, or a Figma Section.
- Only when nothing is selected, ask the user to select something and stop.
- Never modify or delete the original.

## 1.5 Guidelines for judging scope

The following describes recommended behavior. If the user explicitly states the scope or method, follow their instruction instead — treat this as guidance, not a mechanical hard rule.

### Full-page mode

- Use this when the selection is the root frame or a long page frame that makes up the entire page.
- Duplicate the whole page and improve information architecture, section composition, eye-flow, CTAs, and overall rhythm.

### Section mode

- Use this when the selection is a section frame nested inside a page, or a Figma Section.
- Duplicate only the selected target — never modify the original or the parent page.
- Check the parent page and the sections before/after it as reference material, to understand color, typography, width, spacing, image treatment, and information flow.
- Place the improved proposal near the original section so it's easy to compare.
- Preserve the original section's entry and exit points, content width, surrounding spacing, and continuity with the brand expression.
- Never add or change elements outside the selected scope — navigation, footer, other sections, etc.
- Focus on issues specific to this section; don't generalize the same accent to the whole page.

### How improvement directions apply

- **Standard improvement, single proposal**: duplicate the selected section once and make local improvements.
- **Single proposal using cognitive design**: don't force an eye-flow model onto the whole page — use the single most effective structural accent within the selected section.
- **Compare multiple proposals**: not the whole page — line up multiple proposals with different structures using the same section content.
- **Multiple proposals using cognitive design**: create multiple proposals within the selected section that differ in eye-flow path, density, image treatment, and information hierarchy.

Only confirm whether to proceed at the full-page or section level when the interpretation of scope would meaningfully change the deliverable. If the scope can be reasonably determined, proceed without adding extra questions.

## 2. Check for a BRIEF

- Check whether `BRIEF.md`-equivalent content has been attached or provided in the conversation.
- If present, use the target audience, value proposition, differentiation, conversion goal, and brand constraints as grounds for improvement decisions.
- If absent, present two choices: "attach a BRIEF" or "continue without a BRIEF."
- Without a BRIEF, treat the existing design's content as ground truth and never fabricate facts or track records.
- Record whether a BRIEF was available, for use in the optional rewrite step later.

## 3. Check optional companion skills all at once

Before presenting the improvement-direction choices, check in one batch whether the following four custom skills are registered and enabled, and record their availability.

- `cognitive-ui-design`
- `create-multi-pattern`
- `rewrite-me`
- `create-jp-lp`

Right before asking, briefly explain the gist:

> The four items from here on are optional custom skills. You can only choose the corresponding extra options if they're registered and available. Even if none are registered, the standard improvement still runs as-is.

Briefly explain each skill's role too, so a first-time user understands them.

- `cognitive-ui-design`: redesigns information hierarchy and layout based on eye-flow and cognitive traits.
- `create-multi-pattern`: lines up multiple structurally different proposals so they can be compared.
- `rewrite-me`: improves only the copy — headings, body text, CTA labels — to match a BRIEF.
- `create-jp-lp`: applies conventions specific to Japanese commercial LPs — palette, CTA color, image-handling rules — as a diagnostic standard.

Never assume a skill is available when its availability can't be confirmed. In later steps, reuse the result recorded here — never repeat the same existence check.

## 3.1 Choose the improvement direction

Based on the availability of `cognitive-ui-design` and `create-multi-pattern`, present only the option combinations that are actually available. Never show an option that requires an unregistered or disabled skill — this avoids having to ask again after the user has already chosen.

**Never collapse this into a shorter question.** Don't reduce the decision to just "how many proposals?" and relegate the cognitive-design axis to a passing mention in prose instead of an actual choice — that silently removes an available option. Every option below that's currently available must stay individually selectable, whether you ask it as one combined question or as a short sequence of questions (e.g. proposal count first, then whether to apply cognitive design to it). Before creating the proposal, confirm the user was actually able to choose whether cognitive design was applied — not just how many proposals to make.

The base set of options is these four:

1. **Standard improvement, single proposal**
   - Always shown.
   - Makes one improvement proposal that respects the existing design, without using any additional skill.

2. **Single proposal using cognitive design**
   - Shown only if `cognitive-ui-design` is available.
   - Makes one proposal focused on eye-flow, information hierarchy, CTA placement, and structural differentiation.

3. **Compare multiple proposals**
   - Shown only if `create-multi-pattern` is available.
   - Lines up multiple proposals with different structures.

4. **Multiple proposals using cognitive design**
   - Shown only if both `cognitive-ui-design` and `create-multi-pattern` are available.
   - Creates multiple proposals with different eye-flow models.

If the availability of both skills can't be confirmed, present only the standard improvement, and briefly mention that the optional skills' availability couldn't be confirmed if relevant.

## 3.2 Optional Japanese-LP guideline application

Use the `create-jp-lp` availability recorded in Step 3. Don't repeat the same existence check.

If `create-jp-lp` isn't available, skip this step entirely (no question either).

Only when it's available, ask the following. Don't try to infer this heuristically — always confirm with the user.

> AskUserQuestion:
> question: "Is the target a Japanese commercial landing page (LP)?"
> header: "LP check"
> options:
>   - label: "Yes"
>     description: "Apply create-jp-lp's industry × appeal-genre conventions (CTA-color contrast standard, accessibility for baked-in headings, unified plate/card visual language, etc.) as a diagnostic standard when creating the improvement proposal."
>   - label: "No"
>     description: "Create the improvement proposal as usual, with no additional diagnostic standard."

If "Yes," load `create-jp-lp` and use the following as a diagnostic lens (not as a generation procedure — `create-jp-lp`'s Steps 0–1.5 and 6–9 are for generating a single LP from scratch, and don't apply as-is to improving an existing design):

- **Equivalent to Step 4**: whether the CTA button color fits the industry/purpose, and whether the CTA background color and label text color combination meets WCAG AA (4.5:1).
- **Equivalent to Step 5.5 (`plate_style`)**: whether heading image-baking and full-bleed background range fit the appeal genre, whether text placed directly over a photo has a scrim/gradient, whether the same pattern (numbered lists, etc.) is handled consistently across the page, and whether differently-structured cards sharing a row have matched heights.
- **Equivalent to Step 2.5**: whether the composition avoids breaking the assumption that this will later be implemented in Tailwind (e.g. 3+ per-breakpoint background swaps).

Treat any issue found by these criteria as an "observed fact on screen" under Step 4's evidence handling, and report it in the same format as other improvement decisions. Add a short note such as "(Japanese LP guideline)" to the rationale so it's clear the finding came from `create-jp-lp`'s specific criteria.

## 3.5 Optional text rewrite

Use the `rewrite-me` availability recorded in Step 3 and the BRIEF presence recorded in Step 2. Don't repeat the same existence check.

Only show an additional question — after the improvement direction has been decided — when both of the following conditions are met:

- `rewrite-me` is registered and enabled.
- A BRIEF has been attached or provided in the conversation.

When the conditions are met, present these two choices:

1. **Improve the design only; keep the copy as-is**
2. **Improve the design, then also rewrite the copy to match the BRIEF**

If `rewrite-me` isn't registered, isn't enabled, or there's no BRIEF, don't show this question — keep the copy unchanged. Never ask again after presenting an unavailable choice.

If rewriting is selected, apply the `rewrite-me` procedure after the improvement proposal is created, before the final output.

- Never change the original's copy — target only the improvement-proposal side.
- In section mode, target only the copy inside the improvement proposal for the selected section.
- Follow `rewrite-me`'s own procedure for whatever confirmations it requires — rewrite intensity, text volume, scope, etc.
- Never add facts, track record, prices, reviews, guarantees, or testimonials that aren't in the BRIEF.
- Never change layout, color, images, or structure during the rewrite step.
- For multiple proposals, apply the same rewrite policy and content standard to all proposals as a rule, to keep the comparison fair. Only vary the copy per proposal when the user explicitly asks for that.
- Don't output `rewrite-me`'s before/after and caveats as a separate duplicate report — fold them into the final report in Step 8.

## 4. How to handle evidence

When making improvement decisions, split the grounds into these four categories.

1. **Observed facts on screen**
   - Composition, copy, layout, CTAs, images, hierarchy, repetition, gaps, etc. that can actually be confirmed within the selection.

2. **BRIEF or user-provided information**
   - Target audience, purpose, value proposition, brand constraints, conversion goals, etc. that the user has explicitly stated.

3. **General design principles / heuristics**
   - Commonly used design thinking such as eye-flow models, Gestalt principles like proximity and similarity, information hierarchy, cognitive load, banner blindness, etc.
   - Never state a general principle as a universal law. Explain its applicability conditions and its relationship to the current screen.
   - Example: the Z-pattern is commonly used as a composition model that guides the eye from top-left to top-right, then bottom-left to bottom-right, on wide pages. If the current screen has no comparable eye-flow path, adopt it as a candidate for showing key elements in sequence and avoiding monotony — without asserting that every user necessarily reads in a Z shape.

4. **Improvement hypothesis**
   - The expected effect inferred from the above. Always label it explicitly as a "hypothesis," and add a way to verify it where possible.

### Prohibited

- Never fabricate surveys, statistics, sources, user-testing results, competitor information, reviews, or sales impact.
- Never assert unverified effects such as "this will definitely improve" or "conversion rate will go up."
- Never make a mechanical change based on a general principle alone — always tie it back to an observed fact within the selection.
- When a source can't be confirmed, never guess and cite a specific book, researcher, paper, or URL.

## 5. Create the improvement proposal

- For the standard-improvement or cognitive-design single-proposal option, duplicate the original, place it nearby, and edit only the duplicate.
- For multiple proposals, keep the original intact and arrange each proposal in a position where they can be compared.
- Respect the existing brand expression, key copy, product information, and images.
- Improve the following as needed:
  - Section composition and information order
  - Eye-flow and information hierarchy
  - CTAs and the conversion path
  - Elements that help users understand the product/service
  - Whitespace, density, and layout rhythm
  - Information that builds trust, supports comparison, or encourages a return visit
- Never fabricate new facts, track record, prices, reviews, etc. without grounds.
- If a placeholder image or content was reused, state this explicitly in the final report.
- If Step 3.2's Japanese-LP guideline application was "Yes," layer its criteria (CTA color/contrast, `plate_style` — heading image-baking, full-bleed background range, consistent pattern handling, matched card heights within a row) onto the "CTAs and the conversion path" / "Whitespace, density, and layout rhythm" improvement decisions above.

## 5.1 Safe Plugin API writes and transactional creation

- Before writing with the Plugin API, confirm non-obvious property names, accepted values, and object shapes in the local API typings. Do not guess property names.
- For text decoration controls, use the current API names such as `textDecorationOffset`; `textUnderlineOffset` is unsupported and must not be used.
- Treat proposal creation as a transaction. Give temporary and proposal copies deterministic names so a failed current run can identify its own incomplete output safely.
- If a write fails after creating a duplicate, remove only the incomplete copy created by the current run before making at most one materially different retry.
- Never remove or overwrite the original selection, and never delete a previously completed proposal while cleaning up a failed run.
- After a successful write, verify the final proposal's node ID, name, position, key changed properties, and rendered appearance. Confirm that no partial duplicates from the current run remain.

## 6. Choose the final output format

After the improvement (and any optional rewrite) is complete, present these three choices.

1. **Report only**
   - Output an overall summary, structural diff, changes made, evidence, improvement hypotheses, and rewrite diff to the chat.

2. **Annotations only**
   - Attach native Content-category annotations to the key changed/added spots on the improvement-proposal side.
   - If a rewrite was done, include the intent behind the key text changes.
   - Output only a brief completion notice and a link to the target in the chat.

3. **Both report and annotations**
   - Produce both the overall chat report and the per-location annotations on the improvement-proposal side.

## 7. Rules for Content annotations

- Attach annotations only to the improvement-proposal side — never to the original.
- Don't attach them densely across every layer — limit them to key changed/added spots, important CTAs, structural-change targets, and key rewrite locations.
- Attach each annotation to the node that directly corresponds to the change — a section-wide change goes on the corresponding frame; a CTA- or text-specific change goes on that specific node.
- If an annotation can't be attached to a Figma Section itself, attach it to the corresponding inner frame or the changed element instead.
- Reuse the Content category if it already exists. If it doesn't exist, create exactly one category with that name.
- Never delete or overwrite existing unrelated annotations.
- On a re-run, don't add a duplicate annotation with the same content that originated from `hey-polish-me` — update it instead if needed.
- Use the following format for each annotation.

### hey-polish-me
**Change**: what was changed, added, or rewritten
**Observed fact**: the state actually confirmed in the original design
**Basis for the decision**: which of BRIEF, on-screen fact, or general principle it's based on
**Expected effect (hypothesis)**: phrased so it's clear this is unverified
**How to verify**: a reasonable verification approach such as comparative review, user testing, or click-through rate

- When using a general principle, don't just name it — write why it applies to this specific target and under what conditions.
- Never include fabricated sources, numbers, or research results.

## 8. Rules for the report

When "Report only" or "Both report and annotations" is chosen, compare the original with each improvement proposal and report using the following format.

### Improvement overview
- The goal of the improvement
- Scope: full page or section
- The direction adopted
- The BRIEF and skills used
- Whether a text rewrite was done

### Changes and additions
For each item, record:
- The spot changed or added
- Specifically what was changed
- **Observed fact**: the state confirmed in the original design
- **Basis for the decision**: BRIEF, general principle, or both
- **Expected effect**: if unverified, explicitly label it an improvement hypothesis
- **How to verify**: a reasonable verification approach such as comparative review, user testing, or click-through rate

### Text rewrite
Include only if a rewrite was performed.
- The key text targeted
- Before and after
- The BRIEF-based rationale
- Facts/constraints that were preserved
- Layout considerations arising from the change in text volume

### Structural diff
- Full-page mode: number of sections, information flow, main CTAs, sections added/removed
- Section mode: internal composition, information order, CTAs, connection/impact on the sections before and after

### Evidence and confidence
- Observed facts on screen
- Judgments based on the BRIEF
- Judgments based on general principles
- Items treated as hypotheses

### Notes
- Placeholder or reused images/copy
- Content assumed due to insufficient BRIEF information
- Items that need to be replaced or confirmed before production use

Include links to navigate to both the original and the improvement proposal(s).
