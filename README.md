# Sayonara 2027 — Regalia Points Table

A single static page: a public scoreboard that auto-cycles through 5 teams,
plus a passcode-gated panel for the scorekeeper to update points. Scores sync
live across every device that has the page open, using Firebase Realtime
Database (free tier).

## 1. Create a Firebase project (~5 minutes)

1. Go to https://console.firebase.google.com and click **Add project**
   (any name is fine, e.g. "sayonara-2027").
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
     authDomain: "sayonara-2027.firebaseapp.com",
     databaseURL: "https://sayonara-2027-default-rtdb.firebaseio.com",
     projectId: "sayonara-2027",
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
  (defaults to `regalia2027`). Change it to whatever you like before
  deploying.
- **Team names & starting points:** editable from the scorekeeper panel
  itself once the site is live — no need to edit code for this.
- If Firebase isn't configured yet (step 2 skipped), the page still works,
  it just won't sync across devices — each device keeps its own scores.

## Adding your event logo (optional)

Same idea as the video: the page already has a spot for it.

1. In the `assets` folder (same one as the background video, if you're
   using that), add your logo as `logo.png`.
2. A transparent-background PNG or SVG looks best against the dark
   theme — if your logo has a white or solid box background, ask
   whoever made it for a transparent version, or say so here and it
   can likely be worked around.
3. No code changes needed. It'll appear centered at the top of the
   page, in place of the crown emblem. If it's missing, that spot just
   stays empty — nothing breaks.

If you'd rather have it placed somewhere else (e.g. next to the
title), let me know and I'll move it.

## Adding a background video (optional)

The page already has the plumbing for a looping background video — you
just need to add the file:

1. Create a folder called `assets` next to `index.html`.
2. Put your video file in it, named exactly `background.mp4`.
   (Optional: also add a still frame named `background-poster.jpg` in
   the same folder — it shows for a split second while the video loads.)
3. That's it — no code changes needed. If the file isn't there, the
   page just falls back to the plain background, so it's safe to try.

A few practical notes:
- Keep the video **muted** content-wise — the tag is already set to
  autoplay muted and on loop, which is required for autoplay to work
  on phones and most browsers anyway.
- Compress it before adding — aim for well under ~20 MB (1080p,
  H.264 `.mp4`) so it loads quickly on the display device. Handbrake
  (free) is a good tool for this if the raw file is large.
- There's a dark tinted layer over the video by default so the gold
  text stays readable. Once you've picked a video, send it over (or
  describe it) and the tint/color palette can be adjusted to match.
- If you'd rather use a GIF, that works too but tends to be a much
  larger file for the same length of motion — a compressed `.mp4` is
  usually the better choice for a full-screen background.

## Locking it down further (optional)

Test mode leaves the database open to anyone who has the project's
`databaseURL` (not shown publicly, but not truly secret either). For a
short college event this is normally fine — the real gate is that nobody
outside your event has the link or passcode. If you want tighter security
later, look into Firebase's Realtime Database security rules and/or
Firebase Authentication; that's a bigger step so we can tackle it
separately if you decide you need it.
