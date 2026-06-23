# Measure — weekly weight & body tracker

A single-page app for logging your weight and body measurements weekly, with a trend chart. No backend, no build step. All data is saved in your browser via `localStorage`.

## What it does
- Log weight + chest / waist / hips / arm / thigh per entry (weekly, or any cadence)
- Switch units: kg ↔ lb, cm ↔ in (data is stored canonically, so toggling never corrupts it)
- Trend chart with an optional second measurement overlaid on its own axis
- Current weight on a tape-measure scale, plus lowest / highest / change-since-last
- Export / import a JSON backup (your data lives only in this browser, so back it up)

## Deploy to Vercel

**Option A — drag & drop (no tools):**
1. Go to vercel.com → New Project → "Deploy" → drag this folder in.
2. Done. Vercel serves `index.html` automatically.

**Option B — CLI:**
```bash
npm i -g vercel
cd this-folder
vercel        # follow prompts; accept defaults
vercel --prod # promote to production
```

**Option C — Git:** push this folder to a GitHub repo and import it on Vercel. No framework, no build command needed.

## Notes
- It's a static site, so there's nothing to configure — no `vercel.json` required.
- Data is per-browser. To move between devices, use Export on one and Import on the other.
- To add or rename a measurement, edit the form fields and the `MEAS` map in `index.html`.
