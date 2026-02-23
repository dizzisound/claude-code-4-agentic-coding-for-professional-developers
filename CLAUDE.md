# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm run dev       # Start development server (Vite)
npm run build     # Build for production (outputs to docs/)
npm run lint      # Run ESLint
npm run preview   # Preview production build
```

No test runner is configured in this project.

## Architecture

This is a React 19 app built with Vite, styled with PicoCSS, and uses `babel-plugin-react-compiler` for automatic memoization.

**Data flow:** `App.jsx` fetches `public/cast.json` on mount and holds two pieces of state: `cast` (array) and `memberInfo` (selected member object or null). Both are passed down as props to child components; there is no global state manager.

**Component responsibilities:**
- `App.jsx` — root; owns state, fetches data, orchestrates layout
- `Nav.jsx` — top nav with Cast dropdown (renders member links) and `ToggleTheme`
- `ListCast.jsx` — responsive grid of member thumbnail SVGs; clicking opens modal
- `Modals.jsx` — detail modal for a selected member; uses `Arrow` icon and `buttonStyle` from `InterfaceStyles.jsx` for prev/next navigation by array index (`member.id - 1` / `member.id + 1`)
- `ToggleTheme.jsx` — cycles `auto → light → dark`; persists to `localStorage` and sets `data-theme` on `<html>`
- `InterfaceStyles.jsx` — exports a single shared inline style object (`buttonStyle`)

**Assets:** Images are SVGs served from `public/images/`. Each cast member has a full image (`{slug}.svg`) and thumbnail (`{slug}_tn.svg`). The `cast.json` data file is also in `public/`.

**Build output:** Vite is configured with `base: './'` and `outDir: 'docs'`, so `npm run build` writes the production bundle to `docs/` (used for GitHub Pages deployment).
