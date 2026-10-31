# Little Lingo — Learn, Play, Grow

A colorful, fully offline educational quiz and coding-practice app for ages
5–25. No API key, no backend, no server, no internet connection required —
everything runs from a single static `index.html` file with a large
built-in question bank.

## What's in this repo
- `index.html` — the entire app: UI, styles, logic, and a built-in question
  bank of **~13,000 unique questions** across 16 subjects (Math, GK,
  Science, Coding, History, Geography, English, Logical Reasoning, Art,
  Physics, Chemistry, Biology, Civics, Environment, Aptitude, Current
  Affairs), plus a coding-challenge bank (Python, JavaScript, Java, C, SQL).
- `manifest.json`, `sw.js`, `icon-192.png`, `icon-512.png` — make the app
  installable ("Add to Home Screen" / "Install App") once hosted, and let
  it keep working offline after the first load (PWA support).

## How the content works
- Every subject draws from a static bank of questions embedded directly in
  `index.html`. Questions and answer order are shuffled each time, and the
  app actively avoids repeating a question you've just seen (it tracks
  your last 60 seen questions per subject) — once the fresh pool runs low,
  it recycles the oldest-seen ones rather than ever showing a hard stop.
- Difficulty/level determines how many questions you get per round (10 →
  15 → 20 → 25 → 30+, increasing with level).
- Programming challenges work the same way: write your solution, submit,
  and compare against a revealed sample solution — no AI grading, just an
  honesty-based self-check.
- The 📺 button on coding challenges opens a relevant YouTube search in a
  new tab (a plain link out, not an embedded player, so it isn't blocked by
  any content-security policy).

## Running it locally
No build step, no `npm install`, nothing to configure. Just open
`index.html` directly in any modern browser, or serve the folder with any
static file server:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Publishing to GitHub Pages
1. Push this repo to GitHub (see commands below).
2. Go to **Settings → Pages**.
3. Under "Build and deployment", choose **Deploy from a branch**, pick the
   `main` branch and `/ (root)` folder, then **Save**.
4. GitHub gives you a live URL in a minute or two
   (`https://<your-username>.github.io/<repo-name>/`).

```bash
git init
git add .
git commit -m "Initial commit: Little Lingo"
git branch -M main
git remote add origin https://github.com/<your-username>/<repo-name>.git
git push -u origin main
```

## Other free hosting options
- **Netlify Drop**: go to https://app.netlify.com/drop and drag this folder
  in for an instant live link — no account or git needed.
- **Vercel**: `vercel deploy` from this folder.
- **Cloudflare Pages**: connect the GitHub repo, no build command needed.

## Installing it as an app once hosted
- **Android (Chrome):** visit the link → tap "Install app" banner, or menu
  → Add to Home screen.
- **iPhone (Safari):** visit the link → Share icon → "Add to Home Screen."
- **Windows/Mac (Chrome/Edge):** visit the link → click the install icon
  (⊕) in the address bar.

## Growing the question bank further
To keep `index.html` small (about 240 KB), the main `QUESTION_BANK` is stored
gzip-compressed + base64 in the `QUESTION_BANK_PACKED` constant and expanded
once at startup. Each subject is an array of rows in this shape:

```json
["question text", "option A", "option B", "option C", "option D", correctIndex, "optional explanation"]
```

To add questions, decode the string (base64 -> gunzip -> JSON), append rows,
then re-encode and paste it back. Coding challenges are plain JavaScript in
`CODING_BANK`:

```js
{title:"...", description:"...", guidelines:["..."], hint:"...", sampleSolution:"..."}
```

Note: the app needs a modern browser (uses `DecompressionStream`).

## Tech stack
Plain HTML, CSS, and vanilla JavaScript. No frameworks, no bundler, no
dependencies. State (accounts, progress, leaderboard) is kept in the
browser via `localStorage`-style storage, so progress is per-device.

## License
MIT — see `LICENSE`.
