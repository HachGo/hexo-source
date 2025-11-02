# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Hexo static site generator blog repository named "Matrix Dream". The site is configured for Chinese language (zh-CN) and uses the Next theme.

## Development Commands

### Core Commands
- `npm run build` or `hexo generate` - Generate static site files
- `npm run clean` or `hexo clean` - Clean generated files
- `npm run deploy` or `hexo deploy` - Deploy to GitHub Pages
- `npm run server` or `hexo server` - Start local development server

### Content Management
- Create new post: `hexo new "Post Title"`
- Create new page: `hexo new page "Page Title"`
- Create draft: `hexo new draft "Draft Title"`

## Architecture

### Directory Structure
- `source/` - Content source files
  - `_posts/` - Blog posts (Markdown files)
  - `about/` - About page content
  - `categories/` - Category pages
  - `tags/` - Tag pages
  - `images/` - Image assets
  - `index.md` - Homepage content
- `themes/next/` - Next theme files
- `scaffolds/` - Template files for new content
- `public/` - Generated static site (created during build)

### Configuration
- `_config.yml` - Main Hexo configuration
  - Site metadata, URL settings, deployment config
  - Markdown rendering with markdown-it plugins
  - Next theme enabled
- `package.json` - Node.js dependencies and scripts

### Deployment
- Deploys to GitHub Pages via hexo-deployer-git
- Repository: `git@github.com:HachGo/HachGo.github.io`
- Branch: `main`

### Content Creation
- Posts use Markdown with YAML front matter
- Default layout: `post`
- Permalink format: `:year/:month/:day/:title/`
- Supports categories and tags

### Markdown Configuration
- Uses markdown-it with enhanced features
- Plugins enabled: abbr, cjk-breaks, container, emoji, footnote, ins, mark, sub, sup
- Chinese text handling with CJK breaks support
- Typographer enabled for smart quotes and symbols

## Important Notes

- Site language is Chinese (zh-CN)
- Uses Next theme (not the default Landscape theme)
- Custom CNAME file in source directory for domain configuration
- Posts are paginated with 10 posts per page
- Search functionality enabled via hexo-generator-searchdb