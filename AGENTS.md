# AGENTS.md — Agent Guide for fengyun527.github.io

## Project Overview

This is a personal blog built with [Jekyll](https://jekyllrb.com/) and the [`jekyll-theme-chirpy`](https://github.com/cotes2020/jekyll-theme-chirpy/) theme (version ~> 7.5). The site is hosted on [GitHub Pages](https://pages.github.com/) at `https://fengyun527.github.io`.

Blog content is written in Chinese (`lang: zh`), with the timezone set to `Asia/Shanghai`. It records technical learning notes, programming practice, and personal growth reflections.

This repository is derived from the [Chirpy Starter](https://github.com/cotes2020/chirpy-starter) template. The theme is installed as a Ruby gem — theme source files are **not** present in this repository.

## Technology Stack

- **Static Site Generator**: Jekyll (Ruby)
- **Theme**: `jekyll-theme-chirpy` ~> 7.5 (RubyGem)
- **Ruby Version**: 3.4 (used in CI)
- **Markup**: Markdown with YAML front matter
- **Syntax Highlighter**: Rouge
- **Testing**: `html-proofer` ~> 5.0
- **CI/CD**: GitHub Actions
- **Hosting**: GitHub Pages
- **Dev Container**: VS Code Dev Container (`mcr.microsoft.com/devcontainers/jekyll:2-bullseye`)

## Project Structure

```
.
├── _config.yml              # Main Jekyll configuration
├── Gemfile                  # Ruby dependencies
├── index.html               # Homepage (uses `layout: home`)
├── _posts/                  # Blog posts (Markdown)
├── _tabs/                   # Static pages (about, archives, categories, tags)
├── _data/                   # Data files
│   ├── authors.yml          # Author metadata
│   ├── contact.yml          # Sidebar contact options
│   └── share.yml            # Social sharing platforms
├── _plugins/                # Custom Jekyll plugins
│   └── posts-lastmod-hook.rb # Auto-sets last_modified_at from git history
├── assets/                  # Static assets
│   ├── img/                 # Images and favicons
│   └── lib/                 # External libraries
├── tools/                   # Helper scripts
│   ├── init.sh              # One-time environment initialization (from starter)
│   ├── run.sh               # Run local Jekyll server with live reload
│   └── test.sh              # Production build + html-proofer test
├── .github/workflows/       # CI/CD workflows
│   └── pages-deploy.yml     # Build and deploy to GitHub Pages
├── .devcontainer/           # VS Code Dev Container config
└── .vscode/                 # VS Code workspace settings and tasks
```

## Build and Development Commands

### Prerequisites

- Ruby and Bundler
- Install dependencies: `bundle install`

### Local Development

Run the Jekyll development server with live reload:

```bash
./tools/run.sh
```

Or manually:

```bash
bundle exec jekyll serve -l
```

Options for `run.sh`:
- `-H, --host [HOST]` — Bind to a specific host (default: `127.0.0.1`)
- `-p, --production` — Run in `JEKYLL_ENV=production` mode

### Production Build and Test

Build the site for production and run link validation:

```bash
./tools/test.sh
```

This script:
1. Cleans the `_site` directory
2. Builds with `JEKYLL_ENV=production`
3. Runs `htmlproofer` with `--disable-external` and ignores localhost URLs

### VS Code Tasks

Two tasks are defined in `.vscode/tasks.json`:
- **Run Jekyll Server** — `./tools/run.sh`
- **Build Jekyll Site** — `./tools/test.sh`

## Deployment

Deployment is fully automated via GitHub Actions.

**Trigger**: Push to `main` or `master` branch (ignores changes to `.gitignore`, `README.md`, `LICENSE`).

The workflow (`.github/workflows/pages-deploy.yml`) performs:
1. Checkout with full git history (`fetch-depth: 0`) — required for the `posts-lastmod-hook.rb` plugin
2. Setup GitHub Pages
3. Setup Ruby 3.4 with Bundler cache
4. Build: `bundle exec jekyll b -d "_site..."` with `JEKYLL_ENV=production`
5. Test: `bundle exec htmlproofer _site --disable-external ...`
6. Upload artifact and deploy to GitHub Pages

## Content Conventions

### Blog Posts (`_posts/`)

- **Filename format**: `YYYY-MM-DD-title.md`
- **Front matter**:
  ```yaml
  ---
  title: "Post Title"
  description: "Short description"
  date: YYYY-MM-DD HH:MM +0800
  categories: [CATEGORY_NAME]   # uppercase recommended
  tags: [tag_name]              # always lowercase
  ---
  ```
- **Language**: Blog post content is written in Chinese
- **Images**: Place in `assets/img/` and reference with absolute paths: `/assets/img/filename.png`
- **Math**: The Chirpy theme supports MathJax/KaTeX for mathematical notation

### Tabs (`_tabs/`)

Static pages use these front matter fields:
```yaml
---
layout: page      # or archives, categories, tags
icon: fas fa-icon
order: N          # Determines sidebar ordering
---
```

Available tabs in this project:
- `about.md` (`order: 4`) — About page
- `archives.md` (`order: 3`) — Post archives
- `categories.md` (`order: 1`) — Post categories
- `tags.md` (`order: 2`) — Post tags

### Data Files

- **`_data/authors.yml`**: Author information. Key `liuxin` is used.
- **`_data/contact.yml`**: Sidebar contact links (GitHub, Email configured).
- **`_data/share.yml`**: Social sharing buttons (Twitter/X, Facebook, Telegram configured).

## Code Style Guidelines

Follow `.editorconfig`:
- **Charset**: UTF-8
- **Indent**: 2 spaces
- **Line endings**: LF (`\n`)
- **Trailing whitespace**: Trimmed (except in `*.md`)
- **Quotes**: Single for `js/css/scss`, double for `yml/yaml`

## Testing Instructions

The only automated test is `html-proofer`, which validates:
- Internal links are not broken
- Images exist
- HTML is well-formed

External link checking is disabled (`--disable-external`) to avoid flaky CI due to network issues.

## Custom Plugins

### `posts-lastmod-hook.rb`

This plugin registers a Jekyll hook that automatically sets `last_modified_at` for posts based on the git history of the file. It requires full git history (`fetch-depth: 0` in CI), and only applies if a post has more than one commit.

## Security Considerations

- No sensitive data is stored in this repository.
- Comments are disabled globally (`comments.provider` is empty).
- No analytics trackers are configured.
- Webmaster verifications are empty.
- PWA caching is enabled; be cautious with caching sensitive paths if added.

## Dev Container

A VS Code Dev Container is configured (`.devcontainer/devcontainer.json`). It uses the Jekyll image and installs:
- NVM with latest LTS Node.js
- shfmt, zsh-syntax-highlighting, zsh-autosuggestions

Recommended extensions for development are listed in the devcontainer config and include Liquid snippets, Shopify theme check, ShellCheck, shfmt, Prettier, Stylelint, Markdown All in One, and Git Graph.

**Note**: There is currently no `package.json` in this repository. Node.js/npm tooling is available in the devcontainer but is not required for day-to-day development of this site.

## Notes for AI Agents

- Do **not** modify theme files directly — the theme is installed as a gem. Customizations should go in `_config.yml`, custom CSS/JS, or Jekyll plugins.
- When adding new posts, follow the exact filename convention and front matter structure.
- The `_site/` directory is generated and gitignored — never commit it.
- `Gemfile.lock` is gitignored; CI handles dependency locking via `bundler-cache`.
- If adding images, place them in `assets/img/` and reference them with absolute paths starting with `/assets/img/`.
