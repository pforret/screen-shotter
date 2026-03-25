# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a GitHub Actions-based automated screenshot tool built on [shot-scraper](https://github.com/simonw/shot-scraper) (by Simon Willison). It takes screenshots of configured URLs on every push or manual workflow dispatch, then commits the resulting images back to the repo.

## Architecture

- **`shots.yml`** (root): Configuration file listing URLs to screenshot. Each entry has `url`, `output` (filename), and optional `height`. This is the primary file users edit.
- **`.github/workflows/shots.yml`**: GitHub Actions workflow that installs shot-scraper (via Playwright), runs `shot-scraper multi shots.yml`, and auto-commits results.
- **`requirements.txt`**: Single dependency — `shot-scraper`.

## Key Commands

```bash
# Install dependencies locally
pip install -r requirements.txt
shot-scraper install  # installs Playwright browsers

# Take screenshots based on shots.yml
shot-scraper multi shots.yml
```

## How It Works

1. On push or manual trigger, the GitHub Actions workflow runs.
2. If `shots.yml` doesn't exist (first run), the workflow auto-creates it from the repo description URL.
3. `shot-scraper multi shots.yml` processes each entry and saves screenshot files.
4. The workflow commits and pushes any new/changed screenshot files automatically.

## Configuration

Screenshots are configured in `shots.yml` as a YAML list. Full options are documented in the [shot-scraper docs](https://github.com/simonw/shot-scraper#taking-multiple-screenshots). Common fields: `url`, `output`, `height`, `width`, `selector`, `javascript`.
