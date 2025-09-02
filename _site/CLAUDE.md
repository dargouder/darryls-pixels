# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an al-folio Jekyll-based academic website template. Al-folio is a popular Jekyll theme designed for academics to showcase their work, publications, projects, and blog posts. The site is built using Jekyll static site generator with Ruby and includes various plugins for academic features.

## Development Commands

### Local Development
```bash
# Install dependencies
bundle install

# Serve the site locally (with live reload)
bundle exec jekyll serve

# Build the site for production
bundle exec jekyll build

# Format code with Prettier
npx prettier --write .
```

### Docker Development (Recommended)
```bash
# Pull and run with Docker Compose
docker compose pull
docker compose up

# Build and run (if customizing)
docker compose up --build

# Use slim image (beta)
docker compose -f docker-compose-slim.yml up
```

### CSS Optimization
```bash
# Remove unused CSS classes
purgecss -c purgecss.config.js
```

## Architecture and Key Files

### Configuration
- `_config.yml` - Main Jekyll configuration including site settings, plugins, and theme options
- `Gemfile` - Ruby dependencies and Jekyll plugins
- `package.json` - Node.js dependencies (mainly Prettier for formatting)

### Content Structure
- `_pages/` - Static pages (about, projects, publications, CV, etc.)
- `_posts/` - Blog posts in Markdown
- `_projects/` - Project showcase items
- `_news/` - News/announcement items
- `_books/` - Book reviews and reading list
- `_bibliography/papers.bib` - Academic publications in BibTeX format

### Data Files
- `_data/cv.yml` - CV data structure (fallback if JSON resume not found)
- `_data/repositories.yml` - GitHub repositories configuration
- `_data/coauthors.yml` - Publication coauthor information
- `assets/json/resume.json` - JSON Resume format CV data

### Templates and Styling
- `_layouts/` - Liquid templates for different page types (about, post, CV, etc.)
- `_includes/` - Reusable template components
- `_sass/` - SCSS stylesheets organized by component
- `assets/css/main.scss` - Main stylesheet entry point

### Custom Features
- `_plugins/` - Custom Ruby plugins for Jekyll functionality
- `_scripts/` - JavaScript files for interactive features
- Collections support for news, projects, and books
- Responsive image generation with multiple formats (WebP)
- Academic-specific features: citations, bibliography, math typesetting

## Content Management

### Publications
- Edit `_bibliography/papers.bib` for academic publications
- Supports fields like `pdf`, `code`, `website`, `abstract`, `arxiv`, etc.
- Preview images go in `assets/img/publication_preview/`

### Projects
- Add new projects as Markdown files in `_projects/`
- Use front matter to configure project display and categorization
- Images go in `assets/img/` or project-specific subdirectories

### Blog Posts
- Create new posts in `_posts/` with filename format: `YYYY-MM-DD-title.md`
- Supports Distill.pub style posts, math typesetting, and code highlighting
- Can include Jupyter notebooks and various multimedia

### CV
- Primary: Edit `assets/json/resume.json` (JSON Resume standard)
- Fallback: Edit `_data/cv.yml` 
- Configure which sections to display in `_config.yml` under `jsonresume`

## Theme Configuration

### Key Settings in `_config.yml`
- `repo_theme_light/dark` - GitHub stats theme
- `enable_darkmode` - Light/dark mode toggle
- `enable_math` - MathJax for mathematical typesetting  
- `enable_medium_zoom` - Image zoom functionality
- `imagemagick.enabled` - Responsive image generation

### Customization
- Theme colors: Edit `_sass/_themes.scss` and `_sass/_variables.scss`
- Layout modifications: Edit files in `_layouts/` and `_includes/`
- JavaScript features: Add to `_scripts/` and configure in templates

## Deployment

The site is configured for GitHub Pages with automatic deployment via GitHub Actions. Changes to the main branch automatically trigger a build and deploy to the `gh-pages` branch.

For other hosting platforms, use `bundle exec jekyll build` to generate the static site in `_site/`.