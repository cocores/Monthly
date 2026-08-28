# Monthly

A single-page tool for turning a pasted Jira ticket export into a monthly delivery board: a "what shipped this month" report you can review, write up, and share.

Paste ticket rows (tab/comma/pipe-separated, or a raw Jira multi-line export) and it builds:

- **Main board** — items grouped by release, sprint, portfolio, priority, or reporter, with per-item writeups (problem / what shipped / impact to watch) and inline editing of every cell.
- **Dashboard** — priority mix, submitted-by-you split, releases/sprints, team load, and writeup coverage as charts.
- **Updates** — a feed view of every item's writeup plus a free-text monthly summary.
- **Export** — copy a formatted report straight into Confluence (rich HTML, wiki markup, or storage format XHTML), or export Markdown, plain text, CSV, or print/PDF.

Everything runs client-side in one HTML file — no build step, no backend required. Board state lives in memory for the session; use **Save this month** to persist a board to the browser's local storage and reload it later from **Saved months**.

Also has a star-to-save-for-later feature (**Year in review**, for pulling together an end-of-year report) and optional shared team storage — see below.

### Sharing a board with your team

The **Saved months** card has a **Share with your team** section: agree on a code with your teammates (e.g. drop one in Slack), then:

- **Push to team** uploads the current board, your saved months, and your starred items to that code.
- **Pull from team** replaces everything in the browser with whatever was last pushed under that code.

This is a shared code, not a login — anyone who has it can read and overwrite that board, so treat it like a shared link rather than a password.

Shared storage runs on a serverless API route (`api/team.js`) backed by Vercel KV, so it only works on the Vercel deployment, not the static GitHub Pages copy. To enable it on Vercel:

1. In the Vercel dashboard, open this project → **Storage** → **Create Database** → **KV** (or connect an existing one), and link it to the project.
2. Redeploy. Vercel injects `KV_REST_API_URL` and `KV_REST_API_TOKEN` automatically — no other config needed.

Without a KV store linked, Push/Pull show a message explaining shared storage isn't set up yet; everything else in the app works exactly the same.

## Running it

Open `index.html` directly in a browser, or serve it locally:

```
python3 -m http.server 8000
```

then visit `http://localhost:8000`.

## Using it

1. Click **Paste tickets**, fill in the period and your name, and paste rows exported from Jira.
2. Click **Build board** (or **Load sample** to see the shape first).
3. Fill in each item's writeup, add a monthly note, and use **Export** to send the report to Confluence, Markdown, CSV, or print.

## Deploying

The included GitHub Actions workflow (`.github/workflows/deploy.yml`) publishes `index.html` to GitHub Pages on every push to `main`. Enable it under the repo's **Settings → Pages → Source: GitHub Actions**.
