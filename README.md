# ScoreScribe

1. `npm install`
2. Copy `.env.example` to `.env.local` and fill it in (keep your API-Sports key in `API_FOOTBALL_KEY`).
3. Run `supabase/schema.sql` in the Supabase SQL editor.
4. Deploy on Vercel and add the same env vars. The cron in `vercel.json` runs Friday 05:00 UTC (6am WAT).
5. First run, don't wait for Friday:
   `curl -H "Authorization: Bearer $CRON_SECRET" "https://your-domain.com/api/generate?league=premier-league"`
   (omit `?league=` to run every league).

Notes
- Basketball `fixture_id` is stored as 1,000,000,000 + the API id so it can't collide with a football id (the column is unique).
- Each league makes about 30 API-Sports calls per run. Check your plan's per-minute limit.
- Locked tips are stripped on the server; the browser never receives them.
- Vercel cron needs a plan that allows `maxDuration` of 300s for the fan-out; each league runs in its own invocation.
