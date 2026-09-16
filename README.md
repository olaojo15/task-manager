# Capture → Today

A single-file task manager: capture everything into one inbox, promote what matters into
Priority/Today, and archive what's done. No build step, no framework — it's one HTML file
that syncs itself to a JSON file in your own private GitHub repo, so the same board follows
you between your laptop and your phone.

## How it works

- **Capture** — a quick-add bar at the top. Everything lands here first.
- **Priority / Today** — drag a task across (or use the "→ Today" button). Star it to mark
  it a priority; priority tasks float to the top, then it's sorted by due date.
- **Done** — tick a task to archive it. Collapsed by default, sorted newest-first.
- Every task has: text (click to edit inline), an optional due date, and a priority star.
- The board is gated behind a passcode screen, and your task data lives in a GitHub repo
  under your own account, read and written via the GitHub API using a personal access
  token scoped only to that one repo.

**Worth knowing:** GitHub Pages sites from a private (or public) repo are reachable by
anyone who has the exact URL — there's no built-in "only me" access control on free/Pro
plans. The passcode screen is a reasonable deterrent for a personal tool, not bank-grade
security. Don't put anything genuinely sensitive in it, and don't share the URL.

## One-time setup (about 10 minutes)

### 1. Create the repository

1. Go to [github.com/new](https://github.com/new).
2. Name it whatever you like — e.g. `task-manager`.
3. Set it to **Private**.
4. Tick "Add a README file" (or not, it doesn't matter), then **Create repository**.

### 2. Upload the app

1. In the new repo, click **Add file → Upload files**.
2. Upload `index.html` from this folder to the **root** of the repo.
3. Commit directly to `main`.

*(You do not need to upload `data/tasks.json` — the app creates it automatically the
first time you connect.)*

### 3. Turn on GitHub Pages

1. In the repo, go to **Settings → Pages**.
2. Under "Build and deployment", set **Source** to "Deploy from a branch".
3. Set **Branch** to `main` and folder to `/ (root)`, then **Save**.
4. GitHub will give you a URL like `https://<your-username>.github.io/task-manager/`.
   It can take a minute or two to go live the first time.

### 4. Create a personal access token

This lets the app read and write your `tasks.json` file, without giving it access to
anything else in your GitHub account.

1. Go to [github.com/settings/personal-access-tokens/new](https://github.com/settings/personal-access-tokens/new)
   (fine-grained tokens).
2. **Token name**: something like `task-manager-app`.
3. **Expiration**: your call — 90 days or a year is reasonable; you'll just need to
   generate a new one and re-enter it in Settings when it expires.
4. **Repository access**: "Only select repositories" → choose the repo you just created.
5. **Permissions → Repository permissions → Contents**: set to **Read and write**.
   Leave everything else as "No access".
6. Generate the token and **copy it now** — GitHub only shows it once.

### 5. First run

1. Open your GitHub Pages URL on your laptop.
2. You'll land on a setup screen. Fill in:
   - A passcode of your choosing (this device only — you'll set it again on your phone).
   - Your GitHub username.
   - The repository name.
   - Branch: `main`.
   - Data file path: leave as `data/tasks.json`.
   - The token you just copied.
3. Click **Save & connect**. The app creates `data/tasks.json` in your repo and you're in.

### 6. Add your phone (or any other device)

1. Open the same URL on your phone's browser.
2. You'll hit the same setup screen — repeat step 5 with the *same* GitHub repo/token
   details (generate a second token if you'd rather keep them separate; either works).
3. Optionally add the page to your home screen for quick access.

From here, both devices read and write the same `data/tasks.json` file, so a task you
capture on your phone shows up on your laptop next time it syncs (on load, and whenever
you switch back to the tab/app).

## Day to day

- Type into the capture bar, hit Enter — it's in your inbox.
- Drag cards between Capture and Today, or use the small "→ Today" / "← Capture" buttons
  (buttons are more reliable than drag-and-drop on a phone).
- Click the star to flag a priority; click the due-date chip to set or change a date.
- Click a task's text to edit it in place.
- Tick the circle to mark done; it moves into the collapsed Done archive.
- The small dot next to "Settings" in the header shows sync state (green = saved, amber =
  saving, red = couldn't reach GitHub — it'll keep your last synced copy and retry).
- The download icon exports a JSON backup of everything, any time.

## Changing settings later

Click the gear icon in the header to update the repo/branch/path, rotate your token, or
change your passcode on that device. "Forget this device" clears the passcode and GitHub
connection from that browser only — your data in GitHub is untouched.

## If something goes wrong

- **"GitHub PUT failed (401)"** — your token is missing, expired, or wasn't scoped to
  Contents: Read and write. Generate a new one and update it in Settings.
- **"GitHub PUT failed (404)"** — check the username/repo/branch in Settings match
  exactly (case-sensitive).
- **Page loads but nothing syncs** — check you're not on a network that blocks
  `api.github.com`; the app will keep working from its last local copy either way and
  retry automatically.
