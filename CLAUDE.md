# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Personal blog/portfolio site for Pedro Anastácio built with Hugo static site generator.

## Build Commands

```bash
# Install Hugo (macOS)
brew install hugo

# Initialize theme submodule (required after clone)
git submodule update --init --recursive

# Start development server with live reload
hugo server -D

# Build for production (output to /public)
hugo

# Build with drafts included
hugo -D
```

## Architecture

- **Static Site Generator**: Hugo
- **Theme**: Gokarna (git submodule at `themes/gokarna`)
- **Configuration**: `hugo.toml` - site settings, menu, social links
- **Content Directory**: `content/`
  - `index-about.md` - Homepage about section
  - `posts/` - Blog posts (each in own directory with `index.md`)
  - `work/resume.md` - Resume page
- **Static Assets**: `static/` - Favicons, images
- **Build Output**: `public/` - Generated site (committed to repo)

## Content Structure

Blog posts use Hugo's page bundles - each post is a directory containing `index.md` and any related assets. Posts support front matter for metadata (title, date, tags, description).
