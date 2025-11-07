# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Hexo static site generator blog repository named "Matrix Dream" with a two-repository setup for content and source separation. The site uses Chinese language (zh-CN) and the Next theme.

### Repository Architecture

**Two-Repository Model:**
- **`hexo-source`** (this repository) - Contains the Hexo theme, configuration, and static content
- **`hexo-blog-content`** - Contains blog post content (Markdown files)

**Deployment Flow:**
1. When content is updated in `hexo-blog-content`, a GitHub Action workflow triggers a repository dispatch
2. The `deploy.yml` workflow in this repository receives the dispatch signal
3. Content from `hexo-blog-content` is synced to `source/_posts/`
4. The site is built and deployed to GitHub Pages

## Development Commands

### Core Commands
- `npm run build` or `hexo generate` - Generate static site files
- `npm run clean` or `hexo clean` - Clean generated files
- `npm run deploy` or `hexo deploy` - Deploy to GitHub Pages
- `npm run server` or `hexo server` - Start local development server

### Content Management
- `hexo new "Post Title"` - Create new post
- `hexo new page "Page Title"` - Create new page
- `hexo new draft "Draft Title"` - Create draft

## Architecture

### Directory Structure
- `source/` - Content source files
  - `_posts/` - Blog posts (Markdown files) - **populated by CI from hexo-blog-content**
  - `about/` - About page content
  - `categories/` - Category pages
  - `tags/` - Tag pages
  - `images/` - Image assets
  - `index.md` - Homepage content
  - `CNAME` - Custom domain configuration
- `themes/next/` - Next theme files
- `scaffolds/` - Template files for new content
- `public/` - Generated static site (created during build)
- `.github/workflows/deploy.yml` - CI/CD deployment workflow

### Configuration
- `_config.yml` - Main Hexo configuration
  - Site metadata: "Matrix Dream" by Eric hery
  - URL: http://example.com (placeholder)
  - Permalink format: `:year/:month/:day/:title/`
  - Theme: next
  - Pagination: 10 posts per page
- `package.json` - Node.js dependencies and scripts
  - Hexo version: 6.3.0
  - Theme: hexo-theme-landscape (default, but next is configured in _config.yml)

### Deployment
- **Repository:** `git@github.com:HachGo/HachGo.github.io`
- **Branch:** `main`
- **Method:** GitHub Pages via hexo-deployer-git
- **CI/CD:** Automated via GitHub Actions
  - **Trigger:** Repository dispatch from hexo-blog-content
  - **Runner:** ubuntu-latest
  - **Node.js:** 20
  - **Deployment Action:** peaceiris/actions-gh-pages@v3

### CI/CD Workflow Details

**From hexo-blog-content (trigger.yml):**
- Triggered on push to main branch
- Sends repository_dispatch event to hexo-source
- Requires REPO_B_PAT secret

**In hexo-source (deploy.yml):**
1. Checks out hexo-source repository
2. Checks out hexo-blog-content repository using DEPLOY_PAT_NEW
3. Syncs Markdown files from content repo to `source/_posts/`
4. Sets up Node.js 20
5. Caches npm dependencies
6. Installs dependencies with `--legacy-peer-deps`
7. Builds site with `hexo clean` and `hexo generate`
8. Deploys to HachGo/HachGo.github.io via GitHub Pages

### Content Creation
- Posts use Markdown with YAML front matter
- Default layout: `post`
- Supports categories and tags
- Files are synced from hexo-blog-content via CI, not edited directly

### Markdown Configuration

**Renderer:** hexo-renderer-marked (v7.0.0)

**Features:**
- **Preset:** default
- **HTML:** Enabled (parses HTML content)
- **Typographer:** Enabled (smart quotes, symbols)
- **Quotes:** '""'' (converts " to " and ' to ')
- **CJK Breaks:** Enabled for Chinese text handling
- **Anchor Links:** Enabled with custom permalink style

**Plugins Enabled:**
- markdown-it-abbr - Abbreviations
- markdown-it-cjk-breaks - CJK language line breaks
- markdown-it-container - Custom containers
- markdown-it-emoji - Emoji support
- markdown-it-footnote - Footnotes
- markdown-it-ins - Inserted text
- markdown-it-mark - Marked text
- markdown-it-sub - Subscript
- markdown-it-sup - Superscript

**Anchor Configuration:**
- Level: 1 (creates anchors from H1+)
- Permalink: true with '·' symbol on the right
- Case: 0 (no case conversion)
- Separator: '-' (replaces spaces)

## Repository Relationships

```
hexo-blog-content (content)  --[on push]-->  repository_dispatch
                                                      |
                                                      v
hexo-source (theme/config)  <--[fetches content]---  CI builds & deploys
                                                      |
                                                      v
                                            HachGo.github.io (live site)
```

## Important Notes

- **Site language:** Chinese (zh-CN)
- **Theme:** Next (configured in _config.yml)
- **Custom CNAME:** Configured in source/CNAME for domain setup
- **Search:** Enabled via hexo-generator-searchdb
- **Pagination:** 12 posts on index page, 10 posts per archive page
- **Date format:** YYYY-MM-DD
- **Time format:** HH:mm:ss
- **External links:** Open in new tab

## Secret Management

Required GitHub repository secrets:
- `REPO_B_PAT` - Personal Access Token in hexo-blog-content to trigger hexo-source workflow
- `DEPLOY_PAT_NEW` - Personal Access Token in hexo-source to access hexo-blog-content and deploy to GitHub Pages

## Common Development Tasks

**Local Development:**
```bash
# Install dependencies
npm install

# Start local server (http://localhost:4000)
npm run server

# Build site locally
npm run build

# Clean generated files
npm run clean
```

**Content Workflow:**
1. Edit Markdown files in `hexo-blog-content` repository
2. Commit and push to main branch
3. CI automatically syncs content and deploys site
4. No need to edit posts in this repository's `source/_posts/` (they're overwritten by CI)

**Customizing Theme/Config:**
1. Edit `_config.yml` for site configuration
2. Edit files in `themes/next/` for theme customization
3. Create/edit static content in `source/` (about, categories, tags, etc.)
4. Commit changes to trigger deployment
