# Architecture

This project is a **single, self-contained HTML file** (`index.html`). There is no build step, no bundler, no `node_modules`, and no server-side component. All markup, styles, and behavior live in one file and run entirely in the browser.

## File layout

```
.
├── index.html        # The entire application (markup + CSS + JS)
├── assets/
│   └── seth.png      # Floating avatar image ("Dr. G")
├── README.md         # Overview, features, how to view
├── ARCHITECTURE.md   # This file
├── BACKGROUND.md     # Historical/philosophical context and source notes
└── .gitignore
```

## Document structure

`index.html` is organized top to bottom as:

```
<head>
  ├── pre-paint <script>      → adds .js class for progressive enhancement
  └── <style>                 → all CSS, including :root design tokens + dark-mode overrides
<body>
  ├── #progress-bar           → reading-progress indicator (fixed, top)
  ├── #nav                    → sticky header: brand, smooth-scroll links, theme toggle
  ├── .avatar-float           → floating circular avatar "Dr. G" (fixed, fades on scroll)
  ├── #to-top                 → back-to-top button (fixed, bottom-right)
  ├── header.hero             → title, byline, source link, thesis
  ├── div.wrap                → nine <section> blocks (see README table)
  ├── footer
  ├── <script src=mermaid>    → Mermaid 10.9.1 from CDN (pinned)
  └── <script>                → two IIFEs: (1) theme + Mermaid render, (2) dynamic UI
```

## Design system

Follows the **Apple Bridge** token set. No raw hex in component rules — everything references CSS variables defined in `:root` and overridden under `[data-theme="dark"]` and `@media (prefers-color-scheme: dark)`.

| Token | Role |
|---|---|
| `--text-primary` / `--text-secondary` | Body and muted text |
| `--bg-primary` / `--bg-secondary` / `--bg-tertiary` | Surface layers |
| `--border-secondary` | Hairline borders |
| `--accent` / `--accent-soft` | Stoic-gold accent and its tint (used for highlights, the timeline, flip-card backs) |

Type stack is `-apple-system, BlinkMacSystemFont, 'SF Pro Text', sans-serif` at a 17px base, with weights limited to 400/500/600. Spacing follows a 4/8px grid.

## Theming

A pre-paint script in `<head>` adds a `.js` class so reveal animations only hide content when JavaScript is available (no flash for no-JS readers). The theme IIFE:

1. Reads `localStorage['cuf-theme']`, falling back to the OS `prefers-color-scheme`.
2. Applies `data-theme` to `<html>` and swaps the toggle glyph.
3. Re-renders the Mermaid diagram on every theme change (the diagram uses fixed, mode-safe colors via `classDef`, so it reads correctly in both themes).

## Dynamic UI behaviors

All implemented in a single IIFE at the end of `<body>`, and all gated behind `prefers-reduced-motion: reduce`:

- **Reading progress + nav visibility + back-to-top + avatar** — one `scroll` listener, throttled with `requestAnimationFrame`. The nav slides in past ~420px; the back-to-top button appears past ~600px; the floating avatar fades out past ~300px.
- **Smooth scroll** — nav links, the back-to-top button, and in-page jump links (`a.jump`, e.g. the callout's link to the reflection) use `scrollIntoView` / `scrollTo` with `behavior:'smooth'` (or `'auto'` under reduced motion).
- **Scrollspy** — an `IntersectionObserver` with a centered `rootMargin` toggles the `.active` class on the nav link for the section in view.
- **Reveal animations** — logical blocks are auto-tagged with `.reveal` in JS (keeping the markup clean), then revealed once via a second `IntersectionObserver`.
- **Animated counters** — the four stat numbers count up with an ease-out cubic curve when scrolled into view, then settle on their exact display text (which includes glyphs like `½`, `+`, `×` via `data-display`).

## Interactive components

- **Flip cards (BACK US)** — pure CSS. A hidden checkbox plus `:has(input:checked)` drives a 3D `rotateY` flip, so the front (prison directive) and back (business application) work even without JavaScript.
- **Mermaid flowchart** — authored inline in a `<pre class="mermaid">`. The source is preserved in JS so it can be re-rendered on theme toggle. `htmlLabels:true` is set to keep multi-word labels properly spaced; `useMaxWidth:true` makes the SVG scale to the container width.

## Accessibility

- Semantic CSS variables maintain contrast in both modes.
- `prefers-reduced-motion` disables transitions, animations, smooth-scroll, and counter animation; reveal elements are shown immediately.
- Interactive controls (`#themeToggle`, `#to-top`, nav) carry `aria-label`s.
- The `.js` gate ensures content is never hidden when scripts don't run.

## Known constraints

- The Mermaid diagram's node colors are hardcoded in `classDef` (not CSS variables), because Mermaid renders to inline SVG and cannot read the page's custom properties. The chosen values are legible in both light and dark mode.
- A JS-disabled environment (or a preview pane that doesn't execute scripts) shows full layout and text, plus the CSS-only flip cards, but not the Mermaid diagram, counters, scrollspy, reveals, or progress bar.
