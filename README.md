# PDI / MIA Landing Page

One responsive landing page. Desktop visitors see the full site. Phone visitors (screens 760px and narrower) see a focused MVP: hero, two doula cards, the MIA app preview, two payer cards, and both calls to action. The waitlist saves to Supabase. "Book a Demo" opens Calendly.

Everything is already wired. To make the connectors live, you fill in three values and run one Supabase setup step.

## Step 1. Add your three values

Open `config.js` and replace the placeholders:

- `SUPABASE_URL` and `SUPABASE_ANON_KEY`: from your Supabase project, Settings > API. The anon key is meant to be public; its safety comes from Row Level Security, which you turn on in Step 2.
- `CALENDLY_URL`: your Calendly scheduling link, for example `https://calendly.com/yourname/demo`

## Step 2. Set up the Supabase table (one time)

In Supabase, open the SQL Editor and run:

```sql
create table if not exists waitlist (
  id uuid primary key default gen_random_uuid(),
  email text not null,
  role text,
  source text,
  created_at timestamptz default now()
);

alter table waitlist enable row level security;

create policy "public can join waitlist"
  on waitlist for insert
  to anon
  with check (true);
```

This lets visitors add themselves to the waitlist but not read the list. If you already have a waitlist table, make sure it has `email`, `role`, and `source` columns, or adjust the table to match.

## Step 3. Put it on GitHub

Add this whole folder to your repo. On github.com you can drag the files straight into the repo with no command line. Keep `index.html` at the root of the repo.

## Step 4. Deploy on Vercel

1. Go to vercel.com, New Project, import your repo.
2. Framework Preset: **Other**. Leave Build Command and Output Directory empty.
3. Deploy. Vercel serves `index.html` automatically.
4. Add your domain (usepdi.com) under Project Settings > Domains.

After this, every change you push to GitHub redeploys on its own.

## What's in here

- `index.html` — the site, desktop and phone in one file
- `config.js` — your three values
- `images/` — the photos on the page
- `README.md` — this file

## Notes

- Only email, role, and source are collected. No PHI.
- To preview on your own computer, open `index.html` in a browser with the `images` folder next to it.
- Anyone can submit the open waitlist form. If you start seeing spam, add a simple bot check later. Not needed to launch.
