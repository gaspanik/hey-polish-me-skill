# hey-polish-me — Evidence-Based Improvement Proposals for Figma

Register this skill in Figma's custom skill feature, select an already-finished frame or section, and run it from the chat. It analyzes the selection, produces an improvement proposal next to the original (never touching it), and reports its reasoning for every change it makes.

---

## What this is

Once a design is "done," the next question is usually "is it actually good?" — and that's a harder thing for an AI agent to answer honestly. A generic pass tends to either rubber-stamp whatever exists, or confidently invent effects it can't back up ("this will boost conversion").

`hey-polish-me` is built around a stricter discipline: every improvement decision is classified as one of four things — an observed fact on the screen, a requirement the user actually stated (a BRIEF), a general design principle applied with its conditions spelled out, or an explicit, unverified hypothesis. It never fabricates a survey, a statistic, or a guaranteed outcome. The original is always duplicated, never edited in place, so you can compare freely and throw the result away with zero risk.

It works standalone out of the box. If you also have `cognitive-ui-design` (eye-flow/information-hierarchy redesign), `create-multi-pattern` (side-by-side structural comparisons), or `rewrite-me` (BRIEF-aligned copy rewriting) registered, it checks for them up front and unlocks the matching extra options — multiple proposals, structurally distinct layouts, an optional copy rewrite pass. Without them, none of that machinery gets in the way: you get a single, well-reasoned improvement proposal and a report, full stop.

```
Select a finished frame or section
        │
        ▼
Full page, or a section? (root frame → whole-page mode,
nested section/Figma Section → section-only mode)
        │
        ▼
BRIEF attached? → use it as grounds; otherwise treat the
existing design's content as ground truth
        │
        ▼
Check once: are cognitive-ui-design / create-multi-pattern /
rewrite-me registered and enabled?
        │
        ▼
Offer only the improvement-direction options those skills
unlock (standard single proposal always available)
        │
        ▼
Duplicate → improve (never edit the original)
        │
        ▼
Optional: rewrite the copy on the duplicate to match the BRIEF
        │
        ▼
Report / Dev Mode annotations / both — with evidence graded
as fact, BRIEF, principle, or hypothesis
```

---

## Scope

- **Operates on a selection.** Target a finished frame (whole-page mode) or a nested section / Figma Section (section-only mode) — this skill improves an existing design, it doesn't generate one from scratch.
- **Never touches the original.** Every proposal is a duplicate placed near the source; in section mode, the parent page and neighboring sections are read-only reference material, never edited.
- **Graded evidence, not confident guesses.** Every change is tagged as an observed fact, a BRIEF requirement, a general design principle (with its applicability conditions stated), or an explicit hypothesis — never a flat, unverified claim like "this will convert better."
- **No fabrication.** Never invents surveys, statistics, competitor data, user-testing results, or specific sources it can't confirm.
- **Companion skills are optional and auto-detected.** `cognitive-ui-design`, `create-multi-pattern`, and `rewrite-me` are checked once up front; only the option combinations those skills actually unlock are ever shown, so there's no dead-end question about a skill you don't have. Without any of them, the skill still runs a full standard-improvement pass on its own.
- **Copy rewrite is opt-in and BRIEF-bound.** The rewrite pass (via `rewrite-me`) only runs if both `rewrite-me` is available and a BRIEF was provided, and it never adds facts, prices, or testimonials the BRIEF doesn't contain.
- **Flexible output.** Choose a chat report, native Dev Mode annotations (Content category) on the proposal only, or both.

---

## Requirements

- **Figma (Design Agent / custom skill feature).** This skill only uses Figma's built-in Plugin API script execution tool (e.g. `evaluate_script`) and, optionally, other registered custom skills. No external MCP servers or API keys required — register `SKILL.md` as a custom skill and it runs.
- **Optional companion skills**, each unlocking extra options if registered: `cognitive-ui-design`, `create-multi-pattern`, `rewrite-me`. None are required.

---

## Repo structure

```
skills/
  hey-polish-me/
    SKILL.md          — skill definition to register in Figma's custom skill feature
    LICENSE
```

---

## Getting started

**Once published on Figma Community**, you'll be able to add this skill directly from the [AI Skills library](https://www.figma.com/community/ai-skills) — no download needed. Until then, register it from source:

**From source:** clone this repo and register the skill file yourself.

```bash
git clone https://github.com/gaspanik/hey-polish-me-skill
```

**1. Register `skills/hey-polish-me/SKILL.md`** in Figma's custom skill feature.

**2. Select the finished frame or section you want feedback on, then run it from the chat.**

```
/hey-polish-me
```

```
Give me an improvement proposal for this section
```

```
このセクションの改善案を作って
```

---

## Example output

After the improvement proposal is built, the skill reports its reasoning grouped by confidence level:

```
Improvement proposal complete (section mode — "Pricing" section):

Changes:
- Reordered the three pricing tiers to lead with the recommended
  plan (Observed fact: no visual emphasis distinguished the 3
  tiers; BRIEF: "Growth" tier is the target conversion plan)
- Added a comparison table above the CTA (General principle:
  side-by-side comparison reduces choice friction when 3+ options
  exist — Hypothesis: may reduce decision time, verify via
  session recording or A/B test)
- Moved the CTA button above the fold on mobile width
  (Observed fact: CTA required 2 scrolls to reach at 390px width)

Not changed: pricing figures, plan names, and feature lists —
carried over exactly as written, no facts added.

Evidence breakdown: 2 observed facts, 1 BRIEF requirement,
1 general principle (stated with its condition), 1 hypothesis
```

---

## Part of a larger set

This skill is one of 29 Figma-agent skills bundled in **KMRVID Figma Skills** — covering AI-slop-resistant page generation, multi-layout-pattern exploration, layer cleanup, contrast/accessibility checks, tokenization, component audits, ALT text suggestions, and more: [gaspanik.gumroad.com/l/kmrvid-figmaskills](https://gaspanik.gumroad.com/l/kmrvid-figmaskills)

---

Built by Masaaki Komori - [@cipher](https://x.com/cipher) · Skill for [Figma](https://www.figma.com/)
