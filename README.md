# Courage Under Fire — Leadership Edition

![version](https://img.shields.io/badge/version-v1-9A6A1F)
![type](https://img.shields.io/badge/type-single--file%20dashboard-1D1D1F)
![status](https://img.shields.io/badge/status-ready%20for%20distribution-1f7a3f)

An interactive, single-file visual summary of Vice Admiral James Bond Stockdale's 1993 King's College London speech, **"Courage Under Fire: Testing Epictetus's Doctrines in a Laboratory of Human Behavior."** It distills the philosophy that carried Stockdale through more than seven years as a prisoner of war, and translates his **BACK US** framework into lessons for leadership, strategy, team dynamics, and organizational culture.

This is the **Leadership Edition** — adapted for broad distribution to a general audience. Graphic descriptions of torture and self-harm from the original have been trimmed or softened, and specific nationalities have been generalized to "the captors" / "the camp" so the piece reads as a leadership artifact rather than a war narrative. See [BACKGROUND.md](BACKGROUND.md) for the full source context and [ARCHITECTURE.md](ARCHITECTURE.md) for how the page is built.

## What's inside

The page is a guided scroll through nine sections:

| Section | Purpose |
|---|---|
| ⭐ The One Idea | A plain-language summary of the single takeaway |
| 🔬 The Laboratory | Key figures from Stockdale's captivity |
| ✈️ From Cockpit to Cell | Timeline from Stanford (1962) to release (1973) |
| ⚖️ The Dichotomy of Control | The core Stoic framework: what is and isn't "up to you" |
| 💡 What the Philosophy Taught | Four big ideas, led by "shame, not pain, brings a person down" |
| 🎖️ BACK US | Stockdale's survival code, with flip cards mapping each letter to a business domain |
| 🏢 BACK US in the Modern Organization | The reflection: applying the framework to leadership and culture |
| 👥 People in the Story | Epictetus, Rhinelander, Solzhenitsyn, Hatcher & Sybil Stockdale |
| 💬 / ⛓️ Quotes & Invictus | Closing lines and the poem that ends the speech |

## How to view

It's a self-contained HTML file. Open it directly, or serve it locally to enable the interactive features:

```bash
python3 -m http.server 8182
# then open http://localhost:8182/
```

> **Note:** The Mermaid diagram, scroll animations, animated counters, scrollspy, and progress bar are JavaScript-driven. A plain `file://` open or a preview pane that doesn't execute JS will render the layout and text but not those behaviors. Use the local server (above) to see the full experience.

## Features

- **Apple Bridge design system** — semantic CSS variables, SF Pro type stack, 4/8px spacing grid
- **Light & dark mode** — respects OS preference, with a manual toggle persisted to `localStorage`
- **Sticky navigation** with smooth-scroll, scrollspy active-link highlighting, and a reading-progress bar
- **Scroll-triggered reveal animations** and **animated stat counters** (both gated behind `prefers-reduced-motion`)
- **CSS-only flip cards** mapping each BACK US command to a modern business application
- **Mermaid flowchart** bridging the prison framework to the organizational reflection
- **Back-to-top button** and subtle card hover lift
- Fully **responsive** down to mobile widths

## Dependencies

Only one external library, pinned by version and loaded from CDN:

- [Mermaid 10.9.1](https://cdn.jsdelivr.net/npm/mermaid@10.9.1/dist/mermaid.min.js) — renders the flowchart

Everything else (layout, theming, interactivity) is vanilla HTML, CSS, and JavaScript with no build step.

## Source

Stockdale, James Bond. *Courage Under Fire: Testing Epictetus's Doctrines in a Laboratory of Human Behavior.* Hoover Institution on War, Revolution and Peace, Stanford University, 1993. [Original essay (PDF)](https://www.hoover.org/sites/default/files/uploads/documents/StockdaleCourage.pdf)

## License & use

Summary and adaptation for internal leadership-development use. All quotations are drawn from the source text; some wording in this edition has been adapted for general distribution (see BACKGROUND.md).
