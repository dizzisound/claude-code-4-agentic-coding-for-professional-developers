# Copilot Instructions

## Commands

```bash
npm run dev       # Start development server (Vite, localhost:5173)
npm run build     # Build for production → outputs to docs/
npm run lint      # Run ESLint
npm run preview   # Preview production build
```

No test runner is configured.

## Architecture

React 19 app built with Vite, styled with PicoCSS, using `babel-plugin-react-compiler` for automatic memoization (no manual `useMemo`/`useCallback` needed).

**State lives entirely in `App.jsx`** — two state values flow down as props:
- `cast` — array fetched from `public/cast.json` on mount
- `memberInfo` — the currently selected cast member object, or `null`

**Component responsibilities:**
- `App.jsx` — root; owns state, fetches data, orchestrates layout
- `Nav.jsx` — top nav with Cast dropdown and `ToggleTheme`
- `ListCast.jsx` — responsive thumbnail grid; clicking sets `memberInfo`
- `Modals.jsx` — detail modal for selected member; prev/next navigation uses `member.id` as a zero-based array index (`cast[member.id - 1]` / `cast[member.id + 1]`)
- `ToggleTheme.jsx` — cycles `auto → light → dark`; persists to `localStorage`, sets `data-theme` on `<html>`
- `InterfaceStyles.jsx` — exports a single shared inline style object (`buttonStyle`); used by `Modals.jsx`
- `components/icons/Arrow.jsx` — SVG icon component; accepts a `flip` prop to reverse direction

## Key Conventions

- **Shared styles** go in `InterfaceStyles.jsx` as exported plain objects, not CSS classes.
- **Icons** are inline SVG React components under `src/components/icons/`.
- **Images** are SVGs served from `public/images/`. Each cast member has two: `{slug}.svg` (full) and `{slug}_tn.svg` (thumbnail).
- **Data** is in `public/cast.json`. Each member has: `id` (0-based integer), `name`, `slug`, `bio`, `origin`, and nested `favorites`.
- **Build output** is `docs/` (used for GitHub Pages). Don't manually edit files there.
- **No PRs accepted** — this repository does not accept contributions (see CONTRIBUTING.md).
