# Lodestar

A daily morning news brief: markets, energy, geopolitics, business & tech, the Gulf, and my portfolio. Compiled each morning at 7:43 Qatar time from Bloomberg, the FT, the WSJ and Al Jazeera.

## How it works

- `index.html` is the whole app (static, no build step).
- `data/briefs/<YYYY-MM-DD>.json` holds one brief per day.
- `data/briefs/index.json` lists the available dates: `{"dates": ["2026-11-23", ...]}`.
- `data/portfolio.json` lists the holdings shown in the Portfolio tab.

A scheduled Claude task writes each day's brief file, adds its date to `index.json`, and commits.

## Hosting

Settings → Pages → Source: "Deploy from a branch", branch `main`, folder `/ (root)`.
The site will be at `https://<username>.github.io/<repo>/`.

Note: GitHub Pages sites are public.
