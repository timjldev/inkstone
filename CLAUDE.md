# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```sh
npm install        # install dependencies
npm run dev        # start dev server with HMR (http://localhost:5173)
npm run build      # production build (output to dist/), base path forced to /inkstone/
npm run preview    # locally preview the production build
```

There is no lint or test script configured yet.

## Architecture

This is a **Vue 3 + Vite** single-page site (no TypeScript) for an art gallery/portfolio ("inkstone"). No router, no state management, no test framework.

- **Entry**: `index.html` → `src/main.js` → mounts `src/App.vue` on `#app`
- **Page structure**: `App.vue` composes three top-level sections in order — `HeroSection` → `ArtGallery` → `SiteFooter`
- **Components**: live in `src/components/`. Use `<script setup>` (Composition API) for all new components.
- **Path alias**: `@` resolves to `./src` (configured in both `vite.config.js` and `jsconfig.json`)
- **Styles**: global styles in `src/assets/main.css` (imports `base.css`); component-scoped styles use `<style scoped>`

### Artwork data flow

Artwork content is not hardcoded in components — it's data-driven from a static file, editable without touching Vue code:

- `public/artworks.csv` is **pipe-delimited** (`|`), not comma-delimited, with columns `filename|description|fullimage|expanded`. `ArtGallery.vue` fetches and parses it at runtime with a manual `split('|')` (no CSV library).
- Corresponding images live in `public/media/`, named to match the `filename`/`fullimage` columns (e.g. `1.avif`).
- `ArtGallery.vue` renders the parsed rows as a horizontal-scrolling strip of `GalleryItem` components; clicking one opens `ArtworkModal` with the `expanded` description.
- Both `GalleryItem` and `ArtworkModal` build image URLs as `` `${import.meta.env.BASE_URL}media/${filename}` `` — always go through `BASE_URL` rather than a root-relative path, since the site is deployed under a subpath (see below).

### Deployment

Deployed to GitHub Pages via `.github/workflows/deploy.yml` on every push to `main`: `npm ci && npm run build`, then the `dist/` artifact is published through `actions/deploy-pages`. The build is served from `/inkstone/`, which is why `npm run build` passes `--base=/inkstone/` and why components read asset paths through `BASE_URL` instead of assuming root.
