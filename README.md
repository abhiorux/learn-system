# Learn System

A personalized learning management system — local-first, config-driven, privacy-first.

Track what you've learned, get smart review reminders based on the forgetting curve, and explore knowledge through an interactive graph view.

## Features

- **Knowledge Graph** — visual DAG of concepts showing prerequisite dependencies
- **Forgetting Curve Review** — automated review reminders when mastery drops below threshold (τ=7d half-life)
- **Checkpoint & Notes** — mark nodes complete, toggle quick-fix checkpoints, write per-node notes
- **Dark/Light Theme** — toggleable in the stats bar
- **Mobile Responsive** — works on phone and desktop
- **Offline-First** — all data in browser localStorage, no backend, no server, no login

## Quick Start

```bash
# 1. Edit the knowledge base
vim data/knowledge-base.yaml

# 2. Generate data.js
npm run gen

# 3. Open in browser
open src/index.html
```

That's it. No build step, no server, no deployment needed for local use.

## Project Structure

```
├── data/
│   └── knowledge-base.yaml   # Declarative knowledge base config
├── src/
│   ├── index.html            # Main learning UI
│   └── data.js               # Generated from YAML (npm run gen)
├── scripts/
│   └── yaml2js.js            # YAML → JS converter
└── docs/
    └── 01-discussion/        # Design & session notes
```

## How It Works

1. **Knowledge is data** — define concepts, dependencies, and pitfalls in a YAML config file. The system generates the UI from it.
2. **Learn and track** — each node has completion status, comprehension rating, checkpoint flags, and notes.
3. **Review at the right time** — the forgetting curve model `mastery(t) = e^(-t/τ)` flags nodes that need revisiting so you don't lose what you learned.
4. **Explore visually** — the graph view shows the full dependency tree, helping you see what's next and what depends on what.

## Design Principles

- **Config-driven, not hardcoded** — all knowledge lives in YAML, same codebase works for any domain
- **Privacy-first, local computation** — no cloud, no tracking, your learning data stays on your machine
- **Non-intrusive** — quick checkboxes, minimal friction, focus on learning not tooling

## Tech Stack

A single HTML file + generated JS data. Uses vanilla JS, CSS custom properties for theming, and localStorage for persistence. No frameworks, no dependencies at runtime.

## License

MIT
