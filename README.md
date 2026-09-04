# brand-vibes

Give any website the design language of any company - while vibecoding.

`brand-vibes` is an [agent skill](https://agentskills.io) that ships a curated library of **66 full brand design profiles** (Stripe, Linear, Vercel, Apple, Claude, Nike, Ferrari, Spotify, Notion, WIRED, and more) plus a DNA-extraction workflow for companies not in the library. Point your agent at a vibe and it applies the right tokens to your actual codebase - Tailwind v4/v3, plain CSS variables, or shadcn/ui.

## Install

```bash
npx skills add instax-dutta/brand-vibes
```

Works with OpenCode, Claude Code, Codex, Cursor, and [70+ other agents](https://github.com/vercel-labs/skills).

## What it does

Say things like:

- "make my landing page feel like Linear"
- "stripe-vibes for my fintech dashboard"
- "redesign this in the spirit of Claude / Apple / WIRED"

The skill will:

1. **Resolve** the brand from its indexed catalog (or extract the DNA of any off-library company from their live site)
2. **Load only** the matched profile - each is a 9-section design system document: atmosphere, color roles, typography scale, component stylings, layout principles, depth/elevation philosophy, do's and don'ts, responsive behavior, and an agent prompt guide
3. **Distill** it into a semantic token contract (`--bg`, `--accent`, `--font-display`, radius/shadow scales...)
4. **Apply** centrally per stack - never scattered hex values
5. **Verify** against the profile's own don't-list (no pure white on Linear-dark, no bold serif on Anthropic-serif, no cool grays in warm palettes...)

## Library

66 profiles across dev tools, fintech, AI, consumer, editorial, and automotive:

| Mood | Brands |
|------|--------|
| Premium dark dev tool | Linear, Raycast, Sanity, Vercel |
| Warm and human | Claude, Clay, Intercom, Lovable, Zapier |
| Loud and confident | Wise, Revolut, Nike, Bugatti |
| Editorial / magazine | WIRED, The Verge, Cursor |
| Cinematic photo-first | SpaceX, Tesla, Runway, Meta Store |
| Quiet minimal | Ollama, Cal.com, Apple, Uber |
| Trustworthy fintech | Stripe, Wise, Coinbase, Revolut |
| AI startup energy | Replicate, Together AI, Mistral AI, Cohere, xAI |

Full catalog with tags in [`skills/brand-vibes/INDEX.md`](skills/brand-vibes/INDEX.md).

## Plays well with others

Designed to compose, not collide:

- **frontend-design** keeps layout invention and copy voice
- **ui-skills / web-design-guidelines** keep component constraints and final review
- **ui-ux-pro-max** supplies stack recipes; brand-vibes injects the values

Run brand-vibes first so everything downstream styles with the correct tokens.

## Guardrails baked in

- Design language only - never logos, wordmarks, screenshots, or marketing copy from profiled companies
- Free-font substitution table for proprietary typefaces (Söhne → Inter Tight, Anthropic Serif → Source Serif 4, Berkeley Mono → JetBrains Mono...)
- Accessibility floor overrides brand tokens when they conflict (contrast ≥ 4.5:1, visible focus, ≥44px targets, reduced-motion)
- Fidelity ladder: *vibe* → *system* → *pixel-faithful*, so effort matches intent

## License

MIT. Brand profiles are original analytical write-ups describing publicly observable design systems; no proprietary assets are included.

## More agent skills by me

- [flash-compare](https://github.com/instax-dutta/flash-compare) - Flash-style top-1% product comparisons, exactly how flash.co works
- [master-pitcher](https://github.com/instax-dutta/master-pitcher) - Audit, draft, or roast pitch decks with an 18-check VC framework
- [roadmap-tutor](https://github.com/instax-dutta/roadmap-tutor) - Learn any roadmap.sh roadmap one topic at a time, tracked across sessions
- [market-validator](https://github.com/instax-dutta/market-validator) - Validate SaaS ideas with real user complaints across 10+ platforms
- [scroll-3d-world](https://github.com/instax-dutta/scroll-3d-world) - Scroll-scrubbed 3D fly-through landing pages in Three.js, no AI video
- [google-code-review](https://github.com/instax-dutta/google-code-review) - Google's code review best practices as an agent skill
