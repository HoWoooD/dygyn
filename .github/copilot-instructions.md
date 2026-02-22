# Copilot Instructions for Dygyn Games Website

## Project Overview

This is a Hugo static site for displaying results from the Dygyn Games (Игры Дыгына), a multi-sport competition. The site is built with Hugo and the PaperMod theme, deployed to GitHub Pages.

## Build Commands

```bash
# Build the site locally (outputs to ./public)
hugo

# Build with minification (production build)
hugo --minify

# Run local development server with drafts
hugo server -D

# Run local development server (production mode)
hugo server
```

**Hugo Version**: 0.147.2 (extended version, specified in `.github/workflows/hugo.yml`)

## Architecture

### Key Directories

- **`content/posts/`** - Annual results pages (e.g., `2025.md`, `2024.md`)
  - Each file represents one year of competition results
  - Uses shortcodes for winner cards and ranking tables
  
- **`layouts/shortcodes/`** - Custom Hugo shortcodes
  - `winner-card.html` - UFC-style winner card with photo
  - `ranking-table.html` - Responsive ranking table wrapper
  - `sport-tabs.html` - Sport category tabs (for future)
  
- **`assets/css/extended/`** - Custom CSS for PaperMod theme
  - `rankings.css` - All UFC-style rankings styles
  - Includes responsive breakpoints and dark mode support
  
- **`static/images/winners/`** - Winner photos
  - Format: `YYYY-name.jpg`
  - Square images recommended (400x400px or larger)
  
- **`themes/PaperMod/`** - Git submodule for the PaperMod theme
  - **Important**: This is a submodule, not a regular directory
  - Clone with: `git clone --recurse-submodules` or `git submodule update --init --recursive`
  
- **`public/`** - Generated site output (gitignored, created by `hugo` command)

- **`hugo.yaml`** - Main configuration file
  - Sets language to Russian (`ru-ru`)
  - Configures PaperMod theme settings
  - Production environment for analytics and SEO

### Content Structure

Posts use Hugo front matter with TOML format:
```toml
+++
title = '2025'
date = 2026-02-01T16:04:40+09:00
draft = false
Summary = "Победитель: Борис Сдвижков"
+++
```

Content below the front matter includes:
- Winner announcement
- Prize information  
- Results table with columns: №, Участник (participant), event scores, очко (total points), түмүк (final ranking)

## Key Conventions

### Content Language
All content is in **Russian (Sakha language elements)**. Sport names use Yakut/Sakha terms:
- 3х3 (basketball)
- тутум, мас, ох, суруру, хапсаҕай, таас (traditional Yakut sports)

### Custom Shortcodes

#### Winner Card (UFC-style)
Display winner with photo in a gold-bordered card:
```markdown
{{< winner-card 
  name="Борис Сдвижков" 
  year="2025" 
  prize="Главный приз: 3 000 000 ₽" 
  points="30"
  photo="/images/winners/2025-boris-sdvizhkov.jpg"
>}}
```
- Photos go in `/static/images/winners/`
- Photo parameter is optional
- Recommended photo size: 400x400px (square)

#### Ranking Table
Wrap HTML tables for consistent styling:
```markdown
{{< ranking-table sport="Общий зачет" >}}
<thead><tr><th>№</th><th>Участник</th>...</tr></thead>
<tbody><tr><td>1</td><td>Name</td>...</tr></tbody>
{{< /ranking-table >}}
```
- First 3 places get gold/silver/bronze styling
- Fully responsive for mobile
- Hover effects on rows

### Results Tables
Tables follow a strict format with:
1. Ranking number (№)
2. Participant name (Участник)
3. Individual event scores (7 events)
4. Total points (очко)
5. Final placement (түмүк)

Lower total points = better placement (scoring system is inverse).

### New Annual Results
When adding a new year:
1. Create `content/posts/YYYY.md`
2. Use winner-card shortcode for the champion (with photo if available)
3. Use ranking-table shortcode to wrap the results table
4. See `content/posts/2025-example.md` for full template

## Deployment

Site auto-deploys to GitHub Pages on push to `main` branch via `.github/workflows/hugo.yml`. The workflow:
1. Installs Hugo 0.147.2 (extended) and Dart Sass
2. Checks out code with submodules
3. Builds with `hugo --minify`
4. Deploys to GitHub Pages

**Important**: Always commit changes to the main branch to trigger deployment. The site is served from the `public` directory generated during the build.
