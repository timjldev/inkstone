# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```sh
npm install        # install dependencies
npm run dev        # start dev server with HMR (http://localhost:5173)
npm run build      # production build (output to dist/)
npm run preview    # locally preview the production build
```

There is no lint or test script configured yet.

## Architecture

This is a **Vue 3 + Vite** project using plain JavaScript (no TypeScript).

- **Entry**: `index.html` → `src/main.js` → mounts `src/App.vue` on `#app`
- **Components**: live in `src/components/`. Use `<script setup>` (Composition API) for all new components.
- **Path alias**: `@` resolves to `./src` (configured in both `vite.config.js` and `jsconfig.json`)
- **Styles**: global styles in `src/assets/main.css` (imports `base.css`); component-scoped styles use `<style scoped>`

No router, state management, or testing framework is set up — these would need to be added if required.