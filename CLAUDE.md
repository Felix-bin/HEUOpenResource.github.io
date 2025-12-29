# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Static documentation site for Harbin Engineering University course resources, built with VuePress 2.0 and vuepress-theme-plume. The site displays course materials synced from the [heu-icicles repository](https://github.com/HEUOpenResource/heu-icicles).

**Site URL:** https://heu.us.kg

## Development Commands

```bash
# Start dev server with hot reload
npm run docs:dev

# Clean cache and temp, then dev
npm run docs:dev-clean

# Build for production
npm run docs:build

# Preview built site locally
npm run docs:preview
```

**Node.js requirement:** `^20.6.0 || >=22.0.0`

## Architecture

This is a static VuePress 2 site with two main data flows:

### 1. Resource Sync Pipeline
- `update.py` - Fetches README.md files from heu-icicles repo and generates markdown in `docs/resource/`
- `docs/resource/` - Auto-generated content (do not edit manually)

### 2. Contributor Management
- `contributors.json` - Maps course names to non-GitHub contributors
- `addContributors.py` - Injects contributors into markdown frontmatter
- `allContributors.py` - Generates all-contributors list for README

### 3. Build Pipeline
GitHub Actions (`.github/workflows/deploy.yml`):
1. Runs Python sync scripts
2. Builds VuePress site
3. Deploys to gh-pages

## Key Configuration Files

| Path | Purpose |
|------|---------|
| `docs/.vuepress/config.ts` | VuePress main config (GA, CDN, bundler) |
| `docs/.vuepress/plume.config.ts` | Theme config (navbar, footer, comments) |
| `docs/.vuepress/collections.ts` | Auto-generates sidebar from heu-icicles structure |
| `docs/.vuepress/client.ts` | Vue client config, registers custom components |

### Custom Components (in `docs/.vuepress/theme/components/`)
- `Contributors.vue` - GitHub + non-GitHub contributor display
- `Analysis.vue` - Busuanzi visitor statistics
- `OhmmeterTool.vue` - Custom tool component

## Tech Stack

- VuePress 2.0.0-rc.26
- vuepress-theme-plume 1.0.0-rc.181
- TypeScript 5.9.3
- Python 3 + requests (for sync scripts)
- Google Analytics (G-0K3DDDQNDN)
- Giscus comments (linked to heu-icicles repo)
