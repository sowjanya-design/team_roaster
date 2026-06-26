# Turn on the shared board (Supabase) — ~5 minutes

By default the app runs in **Local** mode (data stays in one browser). Follow these
steps once to switch all 10 members onto a single shared, live-syncing board.
There is **no login** — anyone with the link can view and edit, and they pick their
name from the "Acting as" dropdown as before.

## 1. Create a free Supabase project
1. Go to <https://supabase.com> → sign up (free) → **New project**.
2. Pick a name and a strong database password (you won't need it again here).
3. Wait ~1 minute for it to finish provisioning.

## 2. Create the table
In the left sidebar open **SQL Editor → New query**, paste this, and click **Run**:

```sql
-- One row holds the whole shared roster state as JSON.
create table if not exists roster (
  id text primary key,
  data jsonb not null,
  updated_at timestamptz default now()
);

-- No login, so allow the public anon role to read and write this table.
alter table roster enable row level security;

create policy "anon read"   on roster for select using (true);
create policy "anon insert" on roster for insert with check (true);
create policy "anon update" on roster for update using (true) with check (true);

-- Enable realtime so everyone's board updates live.
alter publication supabase_realtime add table roster;
```

## 3. Get your two values
In the sidebar go to **Project Settings → API** and copy:
- **Project URL** — looks like `https://abcdxyz.supabase.co`
- **anon public** key — a long string under "Project API keys" (the `anon` one, **not** `service_role`)

> The `anon` key is meant to be shipped in front-end code — it's safe to commit.
> Never paste the `service_role` key here.

## 4. Paste them into the app
Open `team_roaster.html` (and `index.html`) and near the top of the `<script>` find:

```js
const SUPABASE_URL      = "";   // e.g. "https://abcdxyz.supabase.co"
const SUPABASE_ANON_KEY = "";   // the project's public anon key
```

Fill both in, on **both files** (keep them identical), commit, and push. Vercel
redeploys automatically.

## 5. Verify
Open the live site. The pill in the top bar should now read **● Shared** (green).
Open it in a second browser/phone — make a change in one and watch the other
update within a second.

---

## Notes for a 10-person team
- **Concurrency:** the whole board is saved as one record, so if two people save at
  the exact same second, the later save wins. With 10 people this is very rare and
  fine. (If it ever becomes a problem, the fix is to split into per-task rows.)
- **Anyone with the link can edit.** That's the "no login" trade-off you chose. If
  you later want to gate it, the cheapest step is a shared passcode prompt; the
  proper step is Supabase Auth (real logins). Ask and I'll add either.
- **Reset the board:** in Supabase → Table editor → `roster`, delete the `singleton`
  row. The app reseeds it on next load.
