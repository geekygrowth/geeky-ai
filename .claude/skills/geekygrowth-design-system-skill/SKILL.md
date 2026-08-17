---
name: geekygrowth-design-system-skill
description: "Design system reference for the GeekyGrowth Webflow project. Documents the unprefixed CSS variables, class names, typography scale, colour palette, spacing tokens, radii, mode handling and component variants imported from the Figma file 'GeekyGrowth Studio'. Source of truth: template/style-guide.html and template/styles.css. Use when building new pages, components or UI for GeekyGrowth; when matching the brand; or when the user asks about GeekyGrowth tokens, variables or styling conventions."
---

# GeekyGrowth Design System

Reference for the GeekyGrowth Webflow project. Source of truth: `template/style-guide.html` and `template/styles.css`. Original design source: Figma **GeekyGrowth Studio** (`v9WoUQObfoDYDz1lg6cgH7`), imported 17 August 2026 by reading the four local variable collections directly via the Figma Plugin API — not from a rendered frame.

## Prefix

**This project is unprefixed (MAST-style).** Brand variables are `--purple-100`, `--text-headings`, `--space-md`; base classes are `.button`, `.card`, `.section`. Layout primitives, utilities (`u-*`), combo modifiers (`cc-*`) and page chrome are likewise unprefixed. Do not introduce a prefix.

## Source collections

| Figma collection | Modes | Role |
|---|---|---|
| Brand (67) | Mode 1 | Colour primitives, font families/weights, numeric `Scale` ramp |
| Alias (54) | desktop | Semantic colour roles + border widths |
| Mapped (45) | Default, Invert | Mode-aware Text / Icon / Surface / Border tokens |
| Responsive (96) | Desktop, Tablet, Mobile | Type scale, spacing, radii |

## Webflow-flat selectors (mandatory)

Webflow styles are per-element class lists — it has **no representation** for a descendant (`.a .b`), child (`.a > .b`), or `:nth-child` selector. Author `styles.css` flat so it maps 1:1 onto Webflow:

- Style each element on its own class or a combo chain (`.base.cc-x`). Never style a child through its parent.
- Per-item positions → explicit `.cc-1/.cc-2/…` combos in markup, **not** `:nth-child`.
- A variant that changes a child's layout → a combo on the child (`.container.cc-nav`), not `.nav > .container`.
- **Only allowed descendant:** the mode cascade (`.page-wrapper.cc-invert`), which Webflow reproduces via variable modes.
- Runtime/JS-state rules (`.is-*`, `[data-*]`, `[open]`) belong in the custom-code embed at the foot of `styles.css`.

Self-check: `grep -nE '^\s*\.[a-z][a-zA-Z0-9_-]*(\.[a-zA-Z0-9_-]+)*\s+(\.[a-zA-Z_-]|>|\[)|\s>\s*\.|:nth-child' template/styles.css` should return only comment lines.

## Mode

Two modes: **Default** (the default) and **Invert**. Add `cc-invert` to `.page-wrapper` or `.section` to flip every mapped token. `.section.cc-default` re-asserts Default inside an inverted ancestor. Descendants must consume mapped tokens (`--text-body`, `--surface-page`) rather than literals, or the cascade will not translate to Webflow variable modes.

## Colour tokens

### Primitives (Brand)

| Ramp | Stops |
|---|---|
| Purple | `--purple-05` #f9f8fe · `-10` #f8f7fc · `-20` #e9e4fc · `-40` #d2c9f9 · `-60` #bcadf5 · `-80` #a592f2 · `-100` #8f77ef · `-500` #393060 |
| Lime | `--lime-10` #fdfef9 · `-20` #f8fde8 · `-40` #f1fbd0 · `-60` #e9f9b9 · `-80` #e2f7a1 · `-100` #dbf58a |
| Tomato | `--tomato-10` #fef6f3 · `-20` #fbdbcf · `-40` #f7b7a0 · `-60` #f39370 · `-80` #ef6f41 · `-100` #eb4b11 |
| Pink | `--pink-10` #fffafd · `-20` #ffedf9 · `-40` #ffdaf3 · `-60` #fec8ec · `-80` #feb5e6 · `-100` #fea3e0 |
| Grey | `--grey-10` #f3f3f4 · `-20` #cecdd2 · `-40` #9d9ba6 · `-60` #6d6979 · `-80` #3c384d · `-100` #0b0620 |
| Mono | `--white` #ffffff · `--black` #000000 |

### Semantic roles (Alias) — 6 stops each (10/20/40/60/80/100)

`--primary-*` → Purple · `--secondary-*` → Pink · `--information-*` → Purple · `--action-*` → Lime · `--action-dark-*` → Grey · `--error-*` → Tomato · `--success-*` → Lime · `--neutral-*` → Grey (plus `--neutral-white`, `--neutral-black`).

**Components must reference these, never the raw ramps.**

### Mode-aware (Mapped)

`--text-*`: headings, body, body-secondary, action, action-hover, disabled, information, success, error, on-action
`--icon-*`: same ten roles
`--surface-*`: page, default, action, action-hover, action-dark, action-dark-hover, disabled, information, success, error, pattern-lime, pattern-purple, pattern-tomato, pattern-pink, pattern-dark
`--border-*`: primary, secondary, tertiary, action, action-hover, disabled, information, success, error, dark

Full Default/Invert value table: `template/style-guide.html` → Token index.

> **Note:** Figma spells the token `Border/Tetriary`; the CSS uses the corrected `--border-tertiary`. Rename the Figma variable before the Webflow transfer so the two stay in sync.

## Typography

### Families

| Variable | Stack | Hosting |
|---|---|---|
| `--font-display` | "Bringbold Nineties", Georgia, serif | Webflow CDN (`.woff2`) — Regular + Oblique only, **no bold** |
| `--font-headings` | "Noto Sans", system-ui, … | Webflow CDN (`.ttf`) |
| `--font-body` | "Noto Sans", system-ui, … | Webflow CDN (`.ttf`) |

**Critical:** Webflow registers the condensed faces under the *same* family name `Noto Sans`, reachable by weight. There is no separate "Noto Sans Condensed" family.

| Weight | Face |
|---|---|
| `--weight-regular` 400 | Noto Sans Regular |
| `--weight-condensed-medium` 500 | Noto Sans **Condensed** Medium — used by all headings |
| `--weight-semibold` 600 | Noto Sans SemiBold |
| `--weight-bold` 700 | Noto Sans Bold |

`@font-face` blocks point at the live Webflow CDN, so the static build renders with production files and needs no local assets.

### Size scale (Desktop → Tablet → Mobile, rem)

| Token | Desktop | Tablet | Mobile |
|---|---|---|---|
| `--display-1-size` | 7.5 | 5 | 3.75 |
| `--display-2-size` | 5.375 | 4.25 | 2.9375 |
| `--display-3-size` | 5 | 3.75 | 2.5 |
| `--display-4-size` | 4 | 3 | 2 |
| `--display-5-size` | 3 | 2.5 | 2 |
| `--display-6-size` | 2.5 | 2 | 1.75 |
| `--h1-size` | 5 | 3.75 | 3 |
| `--h2-size` | 3.75 | 3 | 2.5 |
| `--h3-size` | 3 | 2.25 | 2 |
| `--h4-size` | 2 | 1.75 | 1.75 |
| `--h5-size` | 1.5 | 1.5 | 1.5 |
| `--h6-size` | 1.25 | 1.25 | 1.25 |
| `--body-xl-size` | 1.5 | 1.375 | 1.25 |
| `--body-lg-size` | 1.25 | 1.25 | 1.25 |
| `--body-md-size` | 1 | 1 | 1 |
| `--body-sm-size` | 0.875 | 0.875 | 0.875 |
| `--body-xs-size` | 0.75 | 0.75 | 0.75 |
| `--caption-size` | 0.875 | 0.875 | 0.875 |

Line heights are unitless ratios (`--h1-line-height` 1.2, `--h4-line-height` 1.44 desktop / 1.2 below, `--body-md-line-height` 1.5, …). Letter spacing stays in px to match Figma and Webflow 1:1.

### Type classes

- Elements `h1`–`h6` plus `.h1`–`.h6` (so `<h3 class="h2">` works) plus `.text-heading-1`–`-6`
- `.display-1`–`-6` and `.text-display-1`–`-6` (Bringbold)
- `.text-body-xl` / `-lg` / `-md` / `-sm` / `-xs`, each accepting `.cc-semibold`
- `.caption` / `.text-caption` (+ `.cc-semibold`), `.eyebrow` (+ `.cc-action`), `.label`, `.lede`

**Webflow rejects `h1`–`h6` as class names** — anything pushed to the Designer uses the `.text-*` set.

## Spacing

Aliases the numeric `--scale-0` … `--scale-2200` ramp (0 → 360px, converted px→rem at a 16px base).

| Token | Desktop | Tablet | Mobile |
|---|---|---|---|
| `--space-0` | 0 | 0 | 0 |
| `--space-4xs` | 2px | 2px | 2px |
| `--space-3xs` | 4px | 4px | 4px |
| `--space-2xs` | 8px | 8px | 8px |
| `--space-xs` | 12px | 12px | 12px |
| `--space-sm` | 16px | 16px | 16px |
| `--space-md` | 24px | 24px | 20px |
| `--space-lg` | 40px | 32px | 24px |
| `--space-xl` | 60px | 36px | 28px |
| `--space-1x` | 80px | 40px | 40px |
| `--space-2x` | 96px | 56px | 48px |
| `--space-3x` | 120px | 64px | 56px |
| `--space-4x` | 240px | 80px | 80px |
| `--space-margin` | 80px | 40px | 8px |

## Radii & border widths

`--radius-none` 0 · `-2xs` 2 · `-xs` 4 · `-sm` 8 · `-md` 16 (12 mobile) · `-lg` 24 (16 mobile) · `-xl` 40 (24 mobile) · `-2xl` 80 (32 mobile) · `-full` 360px
`--border-width-none` 0 · `-sm` 1px · `-md` 2px · `-lg` 4px

Component aliases: `--radius-button` = `-sm`, `--radius-card` = `-lg`, `--radius-input` = `-sm`, `--button-border-width` = `--border-width-md`.

**Buttons and inputs have no fixed height — deliberately.** Height derives from em-based
padding (`--button-padding-block` / `--button-padding-inline`, default `0.75em` / `1.5em`)
plus font-size, so controls grow with the type under browser zoom or a raised default font
size. `.button.cc-sm` / `.cc-lg` change only `font-size`; the padding scales with it. Never
reintroduce a height token.

## Layout

12-col flexbox grid, unprefixed. `--container-max-width: 90rem` (matches Figma's 1440px desktop frame). Breakpoint cascade: `lg ≥ 992px` · `md ≤ 991px` (Figma Tablet) · `sm ≤ 767px` (Figma Mobile) · `xs ≤ 479px` (inherits Mobile — Figma defines no fourth mode).

**Gotcha:** `.row` pulls `calc(var(--grid-gutter) / -2)` on each side and must never exceed `.container`'s padding. Figma's Mobile `spacing/MARGIN` is only 8px, so `--grid-gutter` is capped at `--space-sm` (16px) below 767px. Raising it reintroduces horizontal page scroll.

## Components

Foundation set only — project components live in `template/components.html` via the `/component` skill.

| Class | Variants |
|---|---|
| `.button` | `cc-dark`, `cc-outline`, `cc-sm`, `cc-lg`, `cc-full`, `cc-disabled` |
| `.card` | `cc-action`, `cc-dark`, `cc-compact`, `cc-flat`; elements `_image` `_title` `_description` (`cc-inverse`) `_footer` |
| `.tag` | `cc-action`, `cc-outline`, `cc-round` |
| `.icon` | `cc-sm`, `cc-lg`, `cc-action`, `cc-headings` |
| `.input` | `cc-error`, `cc-textarea`; plus `.checkbox`, `.radio`, `.form_*` |
| `.section` | `cc-sm`, `cc-lg`, `cc-flush`, `cc-clip`, `cc-invert`, `cc-default` |
| `.container` | `cc-narrow`, `cc-nav`, `cc-footer` |

## Usage rules

- Reference variables via `var(--*)` — never hardcode a hex or px value in a component rule
- Components consume **semantic roles** (`--action-100`) or **mapped tokens** (`--surface-action`), never raw ramps (`--lime-100`)
- Headings are `--weight-condensed-medium` (500) — that weight *is* the condensed face
- Bringbold Nineties has no bold; do not request weight 600+ on display type
- Max 4 utility classes per element; beyond that, promote to a custom class
- British English in copy Claude authors itself. **Never alter user-supplied copy** from Figma, designs or the prompt — preserve spelling, casing and punctuation verbatim
- Do not declare new variables on `:root` once in Webflow — Webflow owns `:root`, and the static `:root` block is deleted at transfer time
- New components via `/component`; re-sync this skill via `/styleguide` (re-sync from code mode) after manual `styles.css` edits
