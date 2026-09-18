# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

**Ready To Deploy** — a free, open-source Python course ("zero to your first deploy") published as a
static, no-build HTML/CSS/vanilla-JS site, available in Portuguese (`pt_br`) and Italian (`it`).
There is no bundler, framework, or build step: pages are plain `.html`/`.css`/`.js` served as-is.
`node_modules/` exists but is not part of the site's runtime.

## Architecture

**Content/template split.** Every top-level page (`index.html`, `pages/*.html`) is a static HTML
shell containing only markup structure + CSS; all text content is fetched at runtime from JSON and
injected via a small inline `<script>` at the bottom of each page. To understand or edit a page's
copy, look in `application_content/<lang>/<page>.json`, not in the HTML.

```
application_content/<lang>/<page>.json   → copy/labels/links for that page (pt_br | it)
course_content/<lang>/<bootcamp>/<module>/*.html → actual lesson/topic content pages
bootcamps/jupyter_notebooks_*/           → source .ipynb notebooks per bootcamp
```

- `<page>.json` files mirror the page's DOM ids/sections 1:1 (e.g. `hero`, `nav`, `modulos-grid`,
  `footer-*`); the page script does `getJSON(APP_CONTENT + '<page>.json')` then populates elements
  by id, and builds cards/lists (modules, projects, bootcamps) from arrays in that JSON.
- `modules.json`, `lessons.json`, `bootcamps.json`, `projects.json` under `course_content/<lang>/`
  are the catalog/index data (what modules exist, their lesson lists, ordering) — separate from the
  page-chrome JSON in `application_content/`.
- Course content itself (the actual lesson HTML pages students read) lives under
  `course_content/<lang>/<bootcamp-slug>/<module-slug>/<topic>.html`, one file per topic, generated
  from notebooks (see below) — not hand-written.

**Language switching** (`assets/lang.js`, loaded in `<head>` right after `assets/theme.js`, no
`defer`) resolves the active language (`?lang=` URL param → `localStorage` → default `pt_br`)
*before first paint*, exposes it as `window.PRIMO_LANG`, and syncs it into the URL. Every page
script reads `window.PRIMO_LANG` to build its `APP_CONTENT`/`COURSE_CONTENT` path prefixes. When
adding a page, follow this same pattern rather than hardcoding language paths.

**Theme** (`assets/theme.js` + `assets/theme.css`) handles dark/light mode, loaded before `lang.js`.

**Content pipeline (course modules).** Course modules are not authored directly as HTML. The
pipeline is: raw material → generated `.ipynb` → published HTML pages + catalog JSON entries.
1. Raw/unstructured material goes in `contributing/raw_content/`.
2. `gerar-modulo-ipynb` skill turns it into a standardized module notebook in
   `contributing/generated_ipynb/modules/module_NN.ipynb`.
3. `gerar-projeto-pratico-ipynb` skill generates the matching hands-on project notebook in
   `contributing/generated_ipynb/projects/project_NN.ipynb`.
4. `gerar-modulo-html` skill publishes an approved module notebook (from
   `bootcamps/<lang>/<bootcamp>/Módulos/module_NN.ipynb`, or from
   `contributing/generated_ipynb/modules/module_NN.ipynb` when reviewing a contribution) into one
   HTML page per topic under `course_content/<lang>/<bootcamp>/<module>/`, and registers it in
   `modules.json`, `lessons.json`, and `bootcamps.json`.

These three are Claude Code skills in `.claude/skills/` — use them (via the `Skill` tool) rather
than hand-writing notebooks or hand-splitting HTML pages when the task is "publish/generate a
module or project."

## Contributing content (non-site changes)

Full guides: [contributing/contribuição.md](contributing/contribuição.md) (pt) and
[contributing/contributo.md](contributing/contributo.md) (it). Summary: contributors don't need to
touch the site at all — they drop raw material in `contributing/raw_content/`, generate the
notebook with the skills above, and the course author publishes it to the site afterward. Content
contributions don't require `node_modules/` or any site build tooling.

## No build/test commands

There is no package.json script, linter, or test suite for the site — it's served directly as
static files (e.g. any static file server pointed at the repo root). Validation for generated
notebooks happens by running them end-to-end in Jupyter/Colab, per the contributing guides.
