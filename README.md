# ┌─────────────────────────────────────────────────────────────┐
# │  Put this file at:  .github/workflows/snake.yml             │
# │  (inside your "Varshith-06" profile repo)                   │
# │                                                             │
# │  Then: repo → Actions tab → "Generate snake" → Run workflow │
# │  Run it ONCE manually. It refreshes itself daily after.     │
# └─────────────────────────────────────────────────────────────┘

name: Generate snake

on:
  schedule:
    - cron: "0 0 * * *" # every day at midnight UTC
  workflow_dispatch: # gives you a manual "Run workflow" button
  push:
    branches: [main]

permissions:
  contents: write

jobs:
  generate:
    runs-on: ubuntu-latest
    timeout-minutes: 10
    steps:
      - name: Generate snake SVGs
        uses: Platane/snk/svg-only@v3
        with:
          github_user_name: Varshith-06
          outputs: |
            dist/github-contribution-grid-snake.svg
            dist/github-contribution-grid-snake-dark.svg?palette=github-dark

      - name: Push SVGs to output branch
        uses: crazy-max/ghaction-github-pages@v4
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
