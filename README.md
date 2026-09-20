# Sangamam 2026 — Regalia Points Table

A single static page: a public scoreboard that auto-cycles through 5 teams,
plus a passcode-gated panel for the scorekeeper to update points. Scores sync
live across every device that has the page open, using Firebase Realtime
Database (free tier).

## 1. Create a Firebase project (~5 minutes)

1. Go to https://console.firebase.google.com and click **Add project**
   (any name is fine, e.g. "sangamam-2026").
2. You can skip Google Analytics when asked.
3. Once the project opens, in the left sidebar go to **Build → Realtime
   Database**, click **Create Database**, choose any region, and start in
   **test mode** (this makes the database open for reading/writing without
   login — fine for a short event; see "Locking it down" below if you want
   more).
4. Back in the project overview, click the **</> (web app)** icon to
   register a new web app. Give it any nickname. You do **not** need
   Firebase Hosting for this step.
5. Firebase will show you a `firebaseConfig` object like:

   ```js
   const firebaseConfig = {
     apiKey: "AIza...",
     authDomain: "sangamam-2026.firebaseapp.com",
     databaseURL: "https://sangamam-2026-default-rtdb.firebaseio.com",
     projectId: "sangamam-2026",
     ...
   };
   ```

## 2. Paste your config into `index.html`

Open `index.html`, search for `FIREBASE_CONFIG`, and replace the
placeholder values with your own `apiKey`, `authDomain`, `databaseURL`,
and `projectId` from step 1.5 above. That's the only code change needed.

## 3. Deploy

**Option A — Vercel (recommended):**
1. Push this folder to a GitHub repo.
2. Go to https://vercel.com, click **Add New → Project**, and import that
   repo. No build settings needed — it's a static file.
3. Deploy. You'll get a URL like `https://your-project.vercel.app`.

**Option B — GitHub Pages:**
1. Push this folder to a GitHub repo.
2. In the repo settings, enable **Pages**, pointing at the branch/root.
3. Your page will be live at `https://yourusername.github.io/reponame`.

Either way, that one URL is what you share: the display device opens it
and just watches; the scorekeeper opens the same URL and taps the crown
icon in the bottom-right corner.

## 4. Using it

- **Passcode:** set in `index.html`, search for `SCOREKEEPER_PASSCODE`
  (defaults to `regalia2026`). Change it to whatever you like before
  deploying.
- **Team names & starting points:** editable from the scorekeeper panel
  itself once the site is live — no need to edit code for this.
- If Firebase isn't configured yet (step 2 skipped), the page still works,
  it just won't sync across devices — each device keeps its own scores.

## Locking it down further (optional)

Test mode leaves the database open to anyone who has the project's
`databaseURL` (not shown publicly, but not truly secret either). For a
short college event this is normally fine — the real gate is that nobody
outside your event has the link or passcode. If you want tighter security
later, look into Firebase's Realtime Database security rules and/or
Firebase Authentication; that's a bigger step so we can tackle it
separately if you decide you need it.
