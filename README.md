# S2A Design Tokens — Style Page

A living visual reference for the `@adobecom/s2a-tokens` design token system used across Adobe.com. The style page renders every token in context — colors, typography, spacing, borders, blur, shadows, and component primitives — with light/dark theme switching and responsive breakpoint preview.

---

## Purpose

This page exists as a single source of truth for developers and designers to:

- Verify token values visually before using them in components
- Confirm light and dark mode token behavior
- Preview responsive typography across Mobile, Tablet, and Desktop breakpoints
- Reference the semantic → primitive token mapping at a glance
- Track which package version the page reflects

---

## Token Source

| Field | Value |
| --- | --- |
| Package | `@adobecom/s2a-tokens` |
| Verified version | `0.0.14` (tarball: `adobecom-s2a-tokens-0.0.14.tgz`) |
| Repository | `github.com/adobecom/consonant` |
| Registry | `npm.pkg.github.com` |
| Last verified | 2026-04-28 |

The page sources tokens from a local `s2a-tokens.css` file — a consolidated hand-maintained mirror of the package's CSS output. When the package updates, this file should be re-synced against the new build artifacts and the Version History table updated.

---

## File Structure

```text
style-page/
├── index.html            # Style page — all sections and JS
├── style.css             # Page-specific styles (demo components, layout)
├── s2a-tokens.css        # Local token mirror (hand-maintained from package)
├── s2a-layout-grids.css  # S2A grid system (container, row, col-* classes)
├── package/              # Pinned package snapshot for reference
│   ├── css/
│   │   ├── dev/          # Per-file token source (primitives, semantic, typography…)
│   │   ├── min/          # Minified combined output
│   │   └── tokens.breakpoints.css
│   ├── CHANGELOG.md
│   └── package.json
└── README.md
```

---

## Local Extensions

The `s2a-tokens.css` file includes a small set of local additions beyond the published package, documented in the file header:

| Extension | Reason |
| --- | --- |
| `--s2a-font-letter-spacing-neg-1` (−1px) | Extra primitive not in 0.0.14 |
| `--s2a-font-letter-spacing-3xl` → `neg-1` | Package maps to `neg-0_96`; local corrects to `neg-1` |
| Full `--s2a-layout-*` scale (sm → 2xl) | Missing from 0.0.14 semantic output |
| Full `--s2a-spacing-*` semantic scale | Retained locally |
| `--s2a-router-card-*`, `--s2a-app-card-*`, `--s2a-product-lockup-*`, `--s2a-layout-rich-media-*` | Design-only in 0.0.14; retained for reference |

---

## Sections

| Section | ID | Description |
| --- | --- | --- |
| Version History | `#version-history` | Package version log and change tags |
| Semantic Colors | `#semantic-colors` | Purpose-driven tokens (content, background, border, focus-ring) with live light/dark toggle |
| Primitive Colors | `#primitive-colors` | Raw color scale — gray, green, blue, red, orange, yellow, transparent black/white |
| Brand Colors | `#brand-colors` | Adobe product identity tokens (`adobe-red`, `cc-ps`, `cc-il`, etc.) |
| Typography | `#typography` | Full responsive type scale with breakpoint slider (Mobile / Tablet / Desktop) |
| Buttons | `#buttons` | All `con-btn` variants, sizes, states, and S2A token anatomy |
| Spacing | `#spacing` | Semantic t-shirt aliases with `var()` mappings and full primitive scale |
| Layout | `#layout` | `--s2a-layout-*` tokens and responsive viewport padding reference table |
| Border | `#border` | Border radius (semantic + primitive) and border width |
| Opacity | `#opacity` | Semantic scrim/disabled aliases and full primitive scale |
| Blur | `#blur` | Semantic blur tokens with backdrop-filter demos and primitive list |
| Shadow | `#shadow` | Four elevation levels with component token breakdown |

---

## Features

### Light / Dark Theme Toggle

A toggle button is pinned to the right of the sticky navigation bar. Activating it sets `data-theme="dark"` on the `<html>` element, which cascades the semantic `--s2a-color-*` tokens into their dark values as defined in `s2a-tokens.css`.

### Responsive Typography Preview

The Typography section includes a three-position slider (Mobile / Tablet / Desktop). Moving the slider applies a scoped CSS class (`text-styles--mobile`, `text-styles--tablet`, `text-styles--desktop`) to the preview container, overriding `--s2a-typography-*` custom properties locally so text renders at the correct breakpoint size regardless of the actual viewport width.

### Sticky Navigation

The nav bar uses `position: sticky` with `backdrop-filter: blur` to float above content while scrolling. Active section state is tracked via `IntersectionObserver`.

---

## Token Architecture

Tokens follow a two-tier structure:

```text
Primitive  →  Semantic  →  Component
─────────────────────────────────────
--s2a-color-gray-1000
    └─▶  --s2a-color-content-default (light)
             └─▶  --s2a-color-button-content-primary-solid-default
```

**Primitives** (`tokens.primitives.css`) are raw values — colors, unitless numbers, pixel values.

**Semantic** tokens (`tokens.semantic.css`, `tokens.semantic.light.css`, `tokens.semantic.dark.css`) assign intent — `content-default`, `background-subtle`, `border-brand`. These split into light and dark sets.

**Responsive** tokens (`tokens.typography.css`, `tokens.typography.desktop.css`, `tokens.typography.tablet.css`) apply breakpoint-specific overrides at `@media (min-width: 1024px)` and `@media (min-width: 1280px)`.

**Component** tokens (`tokens.component.css`) are the most specific — they wire semantic values to individual UI components like buttons and icon buttons.

---

## Updating the Token File

When a new package version is available:

1. Extract the new tarball and locate `css/dev/` and `css/tokens.breakpoints.css`
2. Re-sync `s2a-tokens.css` by merging the updated source files in this order:
   - `tokens.primitives.css`
   - `tokens.semantic.css` + `tokens.semantic.light.css` + `tokens.semantic.dark.css`
   - `tokens.typography.css` (responsive base)
   - `tokens.typography.tablet.css` (≥1024px overrides)
   - `tokens.typography.desktop.css` (≥1280px overrides)
   - `tokens.component.css` (button / icon-button tokens)
3. Re-apply any local extensions noted in the [Local Extensions](#local-extensions) section
4. Add a row to the Version History table in `index.html`
5. Update the `Last verified` date in the `s2a-tokens.css` file header

---

## Token Namespace

All tokens use the `--s2a-` prefix.

```css
--s2a-{category}-{variant}-{modifier}

/* Examples */
--s2a-color-content-default
--s2a-spacing-lg
--s2a-border-radius-md
--s2a-typography-font-size-title-1
--s2a-blur-sm
--s2a-shadow-level-2-blur
```
