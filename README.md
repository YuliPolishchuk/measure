# Measure — weekly weight & body tracker (with Supabase sync)

A single-page app for logging your weight and body measurements weekly, with a
trend chart. Your data now lives in a Supabase database behind a private login,
so it syncs across every device you sign in on. There is still no server code to
write or host — the page talks to Supabase directly.

## What it does
- Log weight + chest / waist / belly / hips / arm / thigh per entry
- "Lost" tile showing total change since your first entry
- Switch units: kg ↔ lb, cm ↔ in (stored canonically, so toggling never corrupts data)
- Trend chart with an optional second measurement on its own axis
- Private email + password login; each account only sees its own data
- Export / import a JSON backup

## One-time Supabase setup

1. Create a free account at supabase.com and click **New project**. Pick a region
   near you (e.g. London / EU West). Wait for it to finish provisioning.
2. Open **SQL Editor → New query**, paste the contents of `supabase-setup.sql`,
   and click **Run**. This creates the `entries` table and locks it to each user.
3. (Recommended for a personal app) Go to **Authentication → Sign In / Providers
   → Email** and turn **off** "Confirm email". That lets you create your account
   and start using it immediately. Leave it on if you'd rather confirm by email.
4. Get your two values. The quickest way is the green **Connect** button at the
   top of the dashboard, which shows both. Or find them manually:
   - **Project URL** — Project Settings → **API** (looks like `https://abcdxyz.supabase.co`)
   - **Key** — Project Settings → **API Keys**. Copy the **Publishable key**
     (starts `sb_publishable_…`). The old **anon / public** key, now under the
     **Legacy API Keys** tab, also works.
5. Open `index.html`, find the SETTINGS block near the top of the `<script>`,
   and paste those two values into `SUPABASE_URL` and `SUPABASE_ANON_KEY`
   (paste whichever key you copied — either works).
6. In **Authentication → URL Configuration**, set **Site URL** to your deployed
   address (and add `http://localhost:3000` if you test locally).

The anon key is meant to live in front-end code — the row-level security rules
from step 2 are what actually keep your data private.

## Deploy to Vercel

Same as a static site — no build step:
- **Drag & drop:** vercel.com → New Project → drop this folder in.
- **CLI:** `npm i -g vercel` then `vercel` and `vercel --prod`.
- **Git:** push to GitHub and import on Vercel.

First time you open it, click **Create account**, then sign in.

## Sharing read-only access with someone

You can let another person view your data without being able to change it.

1. If you already ran the original `supabase-setup.sql`, run **`supabase-sharing.sql`**
   once in the SQL Editor. (Fresh installs: the full `supabase-setup.sql` already
   includes it.)
2. Sign in to the app, scroll to **Share read-only access**, enter the viewer's
   email, and click **Invite**.
3. That person creates their own account using the same email and signs in. They'll
   see your entries, chart, and stats with a "read only" banner — the log form,
   delete buttons, and import are hidden for them.

The block is enforced in the database, not just the page: a viewer's account has
no permission to insert, update, or delete your rows, so they can't change anything
even outside the app. Remove a viewer any time from the same panel.

## Moving your old local data over
If you were using the earlier browser-storage version, open that old version,
click **Export backup**, then in this new version sign in and click **Import** —
it pushes those entries into your Supabase account.

## Notes
- To add or rename a measurement, edit the form fields, the `COLS` list and the
  `MEAS` map in `index.html`, and add a matching column in the SQL.
- Unit and chart preferences are still stored per-device (they're not worth syncing).
