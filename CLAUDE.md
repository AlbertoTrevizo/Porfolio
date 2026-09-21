# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Personal portfolio site for Alberto Trevizo, built with Astro (SSR mode) + Tailwind CSS, deployed on Netlify. It's a single-page site (`src/pages/index.astro`) with anchor-linked sections: About, Experience, Projects, Stack.

## Commands

- `npm run dev` — start local dev server at `localhost:4321`
- `npm run build` — runs `astro check` (type checking) then `astro build`; use this to typecheck the project since there is no separate lint/typecheck script
- `npm run preview` — preview the production build locally
- `npm run astro -- <cmd>` — run any Astro CLI command (e.g. `astro add`)

There is no test suite and no linter configured in this repo.

## Architecture

- **Single-page structure**: `src/pages/index.astro` composes the page from section components (`Experience`, `Projects`, `StackList`) wrapped in `SectionContainer`, which are all rendered inside `src/layouts/Layout.astro`. Navigation in `Header.astro` uses hash links (`#about`, `#experience`, etc.) matching each `SectionContainer`'s `id` prop.
- **Content is hardcoded in components, not in a CMS or content collection.** Data for each section lives as a plain array/object at the top of the relevant `.astro` file's frontmatter:
  - `src/components/Experience.astro` — the `EXPERIENCIE` array (work history)
  - `src/components/Projects.astro` — the `PROJECTS` array and `TAGS` map (tech badges with per-tag color classes)
  - `src/components/StackList.astro` — the `STACK` array (tech icons, sourced from `public/icons/`)
  
  To update site content (add a job, project, or skill), edit these arrays directly rather than looking for a data file.
- **Icons** are individual Astro components under `src/components/icons/` (e.g. `Github.astro`, `LinkedIn.astro`, `NextJS.astro`), each wrapping an inline SVG. Project/stack tag icons referenced from `Projects.astro`'s `TAGS` map follow this same pattern — new tags need a corresponding icon component.
- **Styling**: Tailwind CSS with `darkMode: 'class'`; the layout forces dark mode by wrapping content in a `<div class="dark">`. Content scanning is configured in `tailwind.config.mjs` for `.astro` files under `src/`.
- **Deployment**: configured for Netlify via `@astrojs/netlify` adapter with `output: "server"` in `astro.config.mjs` (SSR, not static output).
