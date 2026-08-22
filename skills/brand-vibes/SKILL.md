---
name: brand-vibes
description: >
  Apply any company's design language to a website while vibecoding. Ships a library
  of 66 full brand design profiles (Stripe, Linear, Vercel, Apple, Claude, Nike,
  Ferrari, Spotify, Notion, and more) plus an off-library DNA-extraction workflow.
  Trigger when the user says "make my site feel like X", "X-style / X-vibe /
  X-aesthetic", "apply X's design system", "redesign in the spirit of X", or names a
  company whose look they want. Resolves the brand, extracts its design DNA into
  semantic tokens, applies them to the user's stack (Tailwind v3/v4, plain CSS vars,
  shadcn/ui), and verifies against the profile's own do/don't rules. Designed to
  compose with frontend-design, ui-skills, web-design-guidelines, and ui-ux-pro-max.
---

# Brand Vibes

Give any website the design language of any company - cleanly, token-first, and without becoming a pixel-clone lawsuit.

## What this skill owns (and what it does not)

This skill owns **the visual DNA**: palette roles, typography scale, radius/shadow/spacing systems, component styling rules, and brand-specific do/don't constraints. It does not own layout invention, copywriting, motion choreography, or accessibility review - other skills own those (see Composition below). When both are loaded, run this skill's workflow FIRST so every downstream skill styles with the correct tokens.

## Library

`brands/` contains one profile per company. Every profile follows the same 9-section anatomy:

| # | Section | Used for |
|---|---------|----------|
| 1 | Visual Theme & Atmosphere | Mood, personality, motion temperament |
| 2 | Color Palette & Roles | Semantic color tokens |
| 3 | Typography Rules | Font families, type scale, weights, tracking |
| 4 | Component Stylings | Buttons, cards, inputs, nav treatments |
| 5 | Layout Principles | Spacing scale, grid, whitespace philosophy |
| 6 | Depth & Elevation | Shadow philosophy and elevation ladder |
| 7 | Do's and Don'ts | Hard constraints - verify against these last |
| 8 | Responsive Behavior | Breakpoints, collapsing strategy, image behavior |
| 9 | Agent Prompt Guide | Ready-made prompts and iteration guide |

Browse `INDEX.md` for the one-line catalog of all 67 brands.

## Workflow

### Step 1 - Resolve the brand(s)

1. Read `INDEX.md`. Match the user's request by name ("linear", "stripe") or by vibe tags ("premium dark dev tool" -> Linear/Raycast/Sanity candidates).
2. One request = one primary brand unless the user explicitly asks for a blend. If they describe a vibe rather than a company, propose your top 2-3 candidates from INDEX.md in one short message and let them pick.
3. Off-library brands: if the requested company is not in `brands/`, extract its DNA yourself from their live site or screenshots, write it into the same 9-section shape (even roughly), then continue at Step 2 identically. Never skip straight to styling.

### Step 2 - Load ONLY the matched profile(s)

Read `brands/<file>.md` for the primary brand (and secondary, if blending). Do not read other profiles - context is precious.

### Step 3 - Extract the DNA into a semantic token sheet

Distill the profile into this stack-agnostic contract before writing any code:

```
Colors      --bg, --bg-elevated, --bg-alt (dark/light section), 
            --text-primary, --text-secondary, --text-muted,
            --accent, --accent-hover, --border-subtle, --border-strong,
            status colors if the profile defines them
Type        --font-display, --font-body, --font-mono + weight/tracking/line-height
            per tier (display, heading, body, caption)
Shape       --radius-sm/md/lg/pill from the profile's Border Radius Scale
Elevation   --shadow-card, --shadow-pop from Section 6 (respect its shadow philosophy:
            ring-based, luminance-stepped, chromatic - whatever it uses)
Rhythm      spacing base + signature values from Section 5
Signature   ONE sentence naming what makes this brand instantly recognizable
            (from Section 1) - this must survive implementation
```

Font substitution rule: never hotlink proprietary fonts. Substitute free equivalents that preserve weight personality and width:

| Proprietary | Free substitute |
|-------------|-----------------|
| Inter Variable | Inter (free) |
| sohne-var | Inter Tight or Archivo |
| SF Pro Display/Text | system-ui stack |
| Anthropic Serif | Source Serif 4 or Lora |
| Berkeley Mono | JetBrains Mono or IBM Plex Mono |
| Geist Sans/Mono | Geist (open source) |
| IBM Plex family | IBM Plex (free) |
| Aeonik Pro | Space Grotesk or Manrope |
| Wise Sans / billboard-weight display | Archivo Black or Anton |

Anything unlisted: pick the closest Google Font matching the profile's stated weight stops and letterform character, and note the substitution to the user.

### Step 4 - Apply to the user's stack

Detect the stack first, then use the matching recipe. Always define tokens once, centrally - never scatter hex values through components.

**Tailwind v4** (CSS-first):

```css
@import "tailwindcss";

@theme {
  --color-bg: <profile bg>;
  --color-surface: <profile elevated>;
  --color-fg: <profile text-primary>;
  --color-fg-muted: <profile text-secondary>;
  --color-accent: <profile accent>;
  --border-subtle: ...;
  --font-display: <family>, <fallback>;
  --font-body: <family>, <fallback>;
  --radius-*: ...;
  --shadow-card: ...;
}
```

**Tailwind v3**: extend `theme.colors`, `theme.fontFamily`, `theme.borderRadius`, `theme.boxShadow` in `tailwind.config.js` with the same semantic names (`bg`, `surface`, `fg`, `fg-muted`, `accent`, `card`, `pop`).

**Plain CSS / any framework**: define the contract verbatim as custom properties on `:root`, then style components against them.

**shadcn/ui**: map into HSL variables - `--background` <- bg, `--foreground` <- text-primary, `--muted-foreground` <- text-secondary, `--primary` <- accent, `--border` <- border-subtle, `--radius` <- the profile's standard card radius. Keep shadcn's variable names; only the values change.

Application order matters: globals/tokens first, then nav + buttons (the two most identity-bearing components), then hero, then cards/sections, then forms/footer. Style each using Section 4's component specs and Section 6's elevation ladder.

### Step 5 - Verify and iterate

Run the checklist before claiming done:

1. Every Section 7 Don't checked? Grep the diff for violations (e.g., pure white on dark brands, bold serif weights, cool grays in warm palettes, pill radii where the brand forbids them).
2. Signature element present? The one-liner from Step 3 should be visibly true on the page.
3. Accessibility floor (overrides the brand when in conflict): body text contrast >= 4.5:1, visible focus states, touch targets >= 44px, `prefers-reduced-motion` respected.
4. Tokens centralized - no stray hardcoded hexes outside the token file.
5. Re-read the profile's Iteration Guide (Section 9) and fix anything it calls out.

## Multi-brand blending protocol

Only when asked. Pick a dominant brand (structure, typography, layout rhythm ~70%) and a supporting brand (one accent color OR one signature element ~30%). Never merge both brands' shadows AND radii AND type - that produces mush. State the split in one line before building.

## Fidelity ladder

Match effort to the user's phrasing:

- "vibe / inspired by / feels like" -> Level 1: colors, type, radius, shadow philosophy only.
- "X-style / X-like design" -> Level 2 (default): full token sheet + Section 4 component treatments + Section 5 layout rhythm.
- "pixel-faithful / exactly like X" -> Level 3: add Section 8 responsive behavior, image treatment, and reuse Section 9's example prompts as build specs.

## Hard guardrails

1. Design language, not assets: never copy logos, wordmarks, product screenshots, illustrations, or marketing copy from the profiled company. No fake "powered by X".
2. The accessibility floor always wins over brand tokens when they conflict - adjust the token value and say so.
3. Inspiration target is the profiled site's public design system, which these documents describe; keep implementations structural and generic.

## Composition with other UI skills

| Skill | Owns | How to compose |
|-------|------|----------------|
| frontend-design | Layout concept, copy voice, signature risk-taking | Run after tokens exist. Its anti-default calibration applies, but the brand profile IS the brief here - its words win. |
| ui-skills / web-design-guidelines | Component constraints, final review pass | Keep active throughout; brand tokens flow through their patterns untouched. |
| ui-ux-pro-max | Stack recipes, charts, UX guidelines | Use its recipes; inject this skill's tokens as the values. |
| cast / paint / motion-design | Motion and art direction | Derive motion temperament from profile Section 1's atmosphere prose. |
| tailwind-patterns / react skills | Implementation mechanics | Orthogonal - no conflict. |
