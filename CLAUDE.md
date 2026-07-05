# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A personal Hugo static blog (Spanish-first, English-translated) built on the [Blowfish](https://blowfish.page) theme (git submodule at `themes/blowfish`), deployed to Firebase Hosting via GitHub Actions on push to `master`. There is no app code here — the repo is content (Markdown), Hugo config (TOML), and thin theme overrides (layouts/assets that shadow the theme).

## Setup & commands

Toolchain is pinned via asdf (`.tool-versions`: golang 1.22.2, hugo extended_0.148.1).

```bash
# one-time setup
git submodule update --init -- themes/blowfish
asdf install

# run dev server (site is NOT at localhost:1313/ root — see language routing below)
hugo server -D

# production build (what CI runs)
hugo --minify -d public
```

### Creating content
Content kind is chosen with `hugo new --kind <archetype>`, and each kind has a Spanish (default) and English (`.en`) archetype variant. Always create both language files for new content.

```bash
# generic post (uses archetypes/default.md)
hugo new content/<section>/my-new-post.md

# cooking recipe
hugo new --kind cooking-recipe recipe/my-dish.md          # ES
hugo new --kind cooking-recipe.en recipe/my-dish.en.md    # EN

# book recommendation
hugo new --kind book-recommendation books/<category>/my-book.md
hugo new --kind book-recommendation.en books/<category>/my-book.en.md
```

### When asked to generate a post
Write the ES + EN Markdown files directly (following the matching archetype's structure/headings) rather than inventing content — ask the user for the type-specific data first if it isn't already given:

- **Recipe** (`content/recipes/<slug>.md` + `.en.md`): summary, servings, prep/cook time, ingredients (with quantities), steps, notes/tips, image filename in `assets/img/recipes/` (author defaults to `tomasmuniz`).
- **Book recommendation** (`content/books/<category>/<slug>.md` + `.en.md`): summary, tema, enfoque, why-read bullets, common objections, ideal/non-ideal reader, key ideas, how to apply them, author entry under `content/authors/`, image filename in `assets/img/books/`.
- **Personal development / generic post** (`content/personal-development/<slug>.md` + `.en.md`, or other `mainSections`): summary, tags/categories, body sections — ask what structure the post needs since this type has no fixed archetype sections.

No separate index/menu file to update per post — sections list their content automatically (see menus in `config/_default/menus.*.toml`, which only link to sections, not individual pages). Setting `draft: false` and filling required front matter is what gets a post into the build (`buildDrafts = false`).

## Architecture / how the pieces fit

**Language routing (bilingual site):** `defaultContentLanguage = "es"` with `defaultContentLanguageInSubdir = true` (config/_default/hugo.toml). Spanish content lives at `content/**/foo.md` and is served under `/es/...`; the English counterpart is the same path with `.en` before the extension (`foo.en.md`), served under `/en/...`. Per-language site params (author bio, social links, date format) live in `config/_default/languages.es.toml` / `languages.en.toml`; per-language menus in `menus.es.toml` / `menus.en.toml`. UI string translations are in `i18n/es.yaml` / `i18n/en.yaml`. When adding a page, section `_index`, or menu entry, both language variants need to be kept in sync or the site will show a broken/missing translation for that language.

**Content model:** three `mainSections` (set in `config/_default/params.toml`): `recipes`, `books`, `personal-development`, plus a non-listed `authors` section used purely as taxonomy/profile pages (referenced from books/recipes via the `authors` front-matter field, e.g. `content/authors/gabormate/`). `books/` is further split into sub-categories (`psychology`, `relationships-and-communication`) — new book categories are just new subdirectories with their own `_index.md`/`_index.en.md`.

**Archetypes drive front matter, not just scaffolding.** `archetypes/cooking-recipe*.md` and `archetypes/book-recommendation*.md` encode the expected structure of that content type (fixed section headings like `# Ingredientes`/`# Paso a paso` for recipes, or `### Tema`/`### Enfoque`/`### Ideas clave` for books) — follow that structure when writing content by hand instead of freeform Markdown, since theme partials and the site's tone rely on it.

**Theme overrides:** `layouts/` and `assets/` at the repo root shadow the same paths inside `themes/blowfish` (standard Hugo override mechanism — nothing here is wired up manually). Custom CSS goes in `assets/css/custom.css` (registered via `params.customCSS` in `config/_default/params.toml`), custom color scheme in `assets/css/schemes/goblin.css` (`colorScheme = "goblin"`). Only touch files under `layouts/`/`assets/` at the root when a theme default needs overriding — check `themes/blowfish/layouts` / `themes/blowfish/assets` first to see what's being shadowed and why, before assuming an override is dead.

**Images:** referenced from front matter (`backgroundImage`, author `image`, etc.) live under `assets/img/{books,recipes,authors,etc}/`, filename-matched to the content slug (e.g. `assets/img/books/el-mito-de-la-normalidad.jpg` backs `content/books/psychology/el-mito-de-la-normalidad.md`).

**Firebase config secret:** `[firebase]` block in `config/_default/params.toml` has an empty `apiKey`, overridden at build time by the CI workflow via `HUGO_PARAMS_FIREBASE_APIKEY` (`.github/workflows/firebase-hosting-merge..yml`). Don't hardcode a real key there.

**Deploy:** any push to `master` triggers the GitHub Action, which builds with `hugo --minify -d public` and deploys `public/` to Firebase Hosting (project `palanx-blog`, see `.firebaserc`/`firebase.json`). There's no separate staging/PR-preview step configured.
