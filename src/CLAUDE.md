<!-- Managed by claude-docs-prompts — do not edit manually -->
<!-- Version: 1.0.0 -->

# Docs Site

This is a Next.js documentation site built with MDX and Tailwind CSS.

**Before making any changes**, read `.docs-config.json` in the repo root for repo-specific values (GitHub repo, branch, production URL, site title, site description). Use these values anywhere the docs reference themselves.

For repo-specific agent instructions that supplement this file, check `.claude/CLAUDE.md` if it exists.

## Project Structure

```
app/              Next.js App Router pages and layouts
components/       React components (UI, navigation, page actions)
config/           Site configuration files (docs.json nav tree, metadata)
content/          MDX documentation pages organized by section
lib/              Utility functions (markdown processing, search, nav helpers)
scripts/          Build and validation scripts (link checker, llms.txt, search index)
public/           Static assets (images, fonts, generated files like llms.txt)
```

## Required Features

Every docs site using this template must implement these seven features.

### 1. PrevNextNav Component

A component that renders previous/next navigation links at the bottom of every docs page.

- Reads the navigation tree from `config/docs.json`
- Derives prev/next pages based on the current page's position in the tree
- Hides "previous" on the first page, "next" on the last page
- Links use the page title as label

### 2. PageActions Dropdown

A dropdown at the top of every docs page with three actions:

- **Copy as Markdown** — copies the raw MDX content of the current page to the clipboard
- **Open in Claude** — opens a new tab with a URL like:
  `https://claude.ai/new?q=Read%20this%20documentation%20page%20and%20help%20me%20understand%20it%3A%0A%0Ahttps%3A%2F%2Fdocs.cyfrin.io%2Fupdraft%2Fwhat-is-a-cyfrin-updraft-professional-certification`
  The URL encodes a prompt asking Claude to read the page, using the page's production URL from `.docs-config.json`.
- **Open in ChatGPT** — opens a new tab with a similar URL format using `https://chatgpt.com/?q=` and the same encoded prompt pattern

### 3. Edit This Page Button

A button/link at the top of every docs page that opens a new tab to the GitHub edit URL for that page's source file. Constructs the URL from `github_repo` and `github_branch` in `.docs-config.json`:

```
https://github.com/{github_repo}/edit/{github_branch}/content/{path-to-file}
```

### 4. Broken Link Checker

A script at `scripts/check-links.ts` that validates all internal links in the MDX content.

- Runs as a GitHub Action on PRs and pushes to the default branch
- Also runnable locally via `pnpm check-links` (or equivalent package.json script)
- Exits with a non-zero code and prints broken links on failure
- Checks both relative links between pages and anchor links within pages

### 5. llms.txt / llms-full.txt Generation

A script at `scripts/build-llms-txt.ts` that generates two files in `public/`:

- **`llms.txt`** — a concise index of all pages with titles and URLs, following the [llms.txt spec](https://llmstxt.org/)
- **`llms-full.txt`** — the full concatenated content of all pages in plain text

Runs as a prebuild script in `package.json` so the files are always fresh at deploy time. Uses `production_url` from `.docs-config.json` for absolute URLs, but relative URLs are good too.

### 6. README.md

Every docs project must include a `README.md` in the repo root that explains how to run the docs locally. At minimum it should cover:

- Prerequisites (Node.js version, package manager)
- Install steps (`pnpm install` or equivalent)
- Dev server command (`pnpm dev` or equivalent) and the local URL
- Build command (`pnpm build`) and how to preview the production build
- Any environment variables or config needed to get started

### 7. Search Index Generation

A script at `scripts/build-search-index.ts` that builds a client-side search index from the MDX content.

- Runs as a prebuild script in `package.json`
- Outputs a JSON search index to `public/` (or a location the search component reads from)
- Indexes page titles, headings, and body content

## Content Organization (Diataxis)

Follow the [Diataxis](https://diataxis.fr/) framework as a guide (not a strict rulebook) for organizing documentation. Docs should aim to include:

- **Quickstart** — get from zero to the "aha" moment as fast as possible
- **Installation guide** — setup and prerequisites
- **Tutorials** — practical activities where the reader learns by doing something meaningful toward an achievable goal
- **How-tos** — task-oriented guides that help the user get something done correctly and safely
- **Reference** — propositional or theoretical knowledge the user looks up during their work
- **Explanation** — content that deepens and broadens understanding, bringing clarity, light, and context

## Technical Conventions

- **Content format**: MDX files in `content/`. Use standard Markdown with JSX components where needed.
- **Styling**: Tailwind CSS utility classes. No custom CSS unless absolutely necessary.
- **Client components**: Add `'use client'` directive only to components that use browser APIs, event handlers, or React hooks like `useState`/`useEffect`. Server components are the default.
- **Icons**: Use `lucide-react` for all icons. Do not add other icon libraries.
- **GitHub Actions**: Pin all actions to full SHA hashes with a version comment: `actions/checkout@<sha> # vX.Y.Z`. Use `persist-credentials: false` on checkout.
- **Dependencies**: Pin exact versions in `package.json` (no `^` or `~` prefixes).
- **Scripts**: All build/validation scripts live in `scripts/` and are written in TypeScript.

<!-- LOCAL CUSTOMIZATIONS — everything below this line is preserved on update -->
