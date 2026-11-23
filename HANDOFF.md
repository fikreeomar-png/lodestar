# Handoff: publish Lodestar to GitHub Pages

## Goal
Publish this folder (the Lodestar daily news brief site) as a **public** GitHub repo named `lodestar` under the GitHub account **fikreeomar-png**, and turn on GitHub Pages so it's live at:

**https://fikreeomar-png.github.io/lodestar/**

The owner has approved a public repo and a public site, including the portfolio data.

## What's in this folder
- `index.html` is the whole app (static, no build step). It fetches the JSON files below with relative paths.
- `data/briefs/index.json` lists the available brief dates: `{"dates": ["2026-11-23"]}`.
- `data/briefs/2026-11-23.json` is the first daily brief.
- `data/portfolio.json` lists the stock holdings shown in the Portfolio tab.
- `.nojekyll` makes Pages serve files as-is. Keep it.
- `README.md` describes the project.

## Steps
1. Unzip `lodestar-github.zip` and `cd` into the `lodestar-gh` folder.
2. Create the repo and push:
   ```bash
   git init -b main   # skip if .git already exists
   git add -A
   git commit -m "Lodestar: daily news brief site"
   gh repo create fikreeomar-png/lodestar --public --source=. --remote=origin --push
   ```
   If the repo already exists, use `git remote add origin https://github.com/fikreeomar-png/lodestar.git && git push -u origin main` instead.
3. Turn on GitHub Pages from `main` at the repo root:
   ```bash
   gh api -X POST repos/fikreeomar-png/lodestar/pages \
     -f "source[branch]=main" -f "source[path]=/"
   ```
   If that returns 409, Pages is already on; that's fine.
4. Wait about 1–2 minutes, then check the site:
   ```bash
   gh api repos/fikreeomar-png/lodestar/pages --jq .status
   curl -sI https://fikreeomar-png.github.io/lodestar/ | head -1
   curl -s https://fikreeomar-png.github.io/lodestar/data/briefs/index.json
   ```
   The site is live when the status is `built` and the curl returns HTTP 200.
5. Report the live URL back to the user.

## Don't
- Don't rename or move files: `index.html` loads `data/...` by relative path.
- Don't add a build step, framework or Jekyll config.
- Don't make the repo private: GitHub Pages on a free account needs a public repo.

## After it's live
Daily updates come from a separate scheduled Claude task (7:43am Qatar time). Each day it adds `data/briefs/<YYYY-MM-DD>.json`, appends that date to `data/briefs/index.json`, and commits to `main`. The user will go back to the original chat to connect that task to this repo. Nothing more is needed here.
