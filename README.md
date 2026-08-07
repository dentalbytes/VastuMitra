# Vastu Guide

A room-by-room reference guide and toolkit for traditional Vastu Shastra — covering the main entrance, living room,
kitchen, bedroom, bathroom, pooja room, staircase, study room, dining room, and balcony/garden — plus a searchable
Plants & Trees guide and a set of interactive tools (checklists, score card, compass, advisor, and more).

The site draws on both North and South Indian schools of thought, calling out the specific points where regional
practice tends to differ, and leads with a clear disclaimer that Vastu Shastra is a traditional belief system, not
an empirically validated science.

## Tech stack

Plain HTML, CSS, and vanilla JavaScript for the entire site — no build step, no framework, no `npm install`
required to run it. Styling uses the [Tailwind CSS Play CDN](https://tailwindcss.com/docs/installation/play-cdn)
loaded directly in the browser. Every interactive tool (checklists, compass, advisor, plot rating, etc.) runs
client-side using `localStorage` — no account system, no data leaves the visitor's browser.

Two features — the **AI Consultant** and **Floor Plan Analysis** — are the exception: they call small serverless
functions in `api/` that talk to an AI provider. Those need a one-time setup step (see below) and only work once
deployed to a host that runs serverless functions (Vercel does this automatically; plain GitHub Pages does not).

```
vastu-guide/
├── index.html               Home page — room grid, tools grid, intro
├── entrance.html … balcony-garden.html    10 room guides
├── plants.html               Plants & trees guide (searchable/filterable)
├── disclaimer.html           About & full disclaimer
├── roadmap.html              What's live, what needs setup, what's not built (and why)
│
├── room-checker.html         Interactive per-room checklist
├── score-card.html           Combined score across checked rooms
├── compass.html              Interactive 8-direction compass (+ live device-compass mode)
├── dashboard.html            Direction ↔ life-aspect dashboard
├── advisor.html              Rule-based personalized suggestions (no AI needed)
├── daily-tips.html           Traditional tip of the day
├── remedies.html             Searchable non-structural remedies library
├── plot-selection.html       Rule-based plot rating calculator
├── rent-checker.html         5-minute tenant checklist
├── offices.html              Vastu for commercial/office spaces
├── products.html             Educational guide to common remedy objects (no shop/links)
├── progress.html             Gamification badges ("My Progress")
├── quiz.html                 12-question Vastu Knowledge Quiz
├── ask.html                  FAQ + "ask a question" form (opens the visitor's email app, no backend)
│
├── consultant.html           AI Consultant chat (BETA — needs setup, see below)
├── floor-plan.html           Floor plan upload & analysis (BETA — needs setup, see below)
├── api/
│   ├── consult.js             Serverless function behind the AI Consultant
│   └── analyze-floorplan.js   Serverless function behind Floor Plan Analysis
│
├── css/style.css             Small amount of custom CSS on top of Tailwind
├── js/main.js                Shared header/footer/nav (Rooms + Tools menus)
├── js/badges.js              Gamification badge logic
├── js/checker-data.js        Room checklist items + remedies
├── js/compass-data.js        8-direction data (compass + dashboard + daily tips)
├── js/daily-tips-data.js     Weekday tip content
├── js/remedies-data.js       Remedies library data
├── js/quiz-data.js           Quiz questions, answers, and explanations
├── js/plants-data.js         Plant data used by plants.html
└── vercel.json
```

## Previewing locally

No build tools needed for the core site. Any of these work:

**Option 1 — just open it**
Double-click `index.html`, or open it in your browser directly. (The `api/` features won't respond locally this
way — see below.)

**Option 2 — local server (recommended, avoids file:// quirks)**
```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

**Option 3 — Vercel dev (to test the AI features locally too)**
```bash
npm install -g vercel   # one-time
vercel dev
```

## Deploying to GitHub

1. Create a new repository on GitHub (don't initialize it with a README — you already have one).
2. From inside this folder:
   ```bash
   git init
   git add .
   git commit -m "Initial commit: Vastu Guide"
   git branch -M main
   git remote add origin https://github.com/<your-username>/<your-repo-name>.git
   git push -u origin main
   ```

## Deploying to Vercel

**Option 1 — Vercel dashboard (no CLI needed)**
1. Push the repo to GitHub first (see above).
2. Go to [vercel.com/new](https://vercel.com/new) and import the GitHub repository.
3. Vercel auto-detects the static pages and the `api/` serverless functions — no build command or output
   directory needed. Click **Deploy**.

**Option 2 — Vercel CLI**
```bash
npm install -g vercel   # one-time
cd vastu-guide
vercel                  # follow the prompts
vercel --prod           # deploy to production
```

## Setting up the AI Consultant & Floor Plan Analysis (optional)

These two features are built but inert until you add an API key — without one they show a friendly "not set up
yet" message instead of erroring.

1. Get an API key from either:
   - **Anthropic**: [console.anthropic.com](https://console.anthropic.com) → `ANTHROPIC_API_KEY`
   - **OpenAI**: [platform.openai.com](https://platform.openai.com) → `OPENAI_API_KEY`
   (Only one is required — if both are set, Anthropic is used.)
2. In your Vercel project: **Settings → Environment Variables** → add the key.
3. Redeploy (Vercel → Deployments → ⋯ → Redeploy, or just push a new commit).

That's it — no code changes needed. Both `api/consult.js` and `api/analyze-floorplan.js` pick up whichever key is
present. Keep in mind actual usage costs money on a pay-as-you-go basis with either provider, and there's no rate
limiting built in — consider adding some (e.g. via Vercel's built-in protections) before sharing the link widely.

## Customizing content

- **Room pages**: each room is its own self-contained HTML file with "Traditionally Recommended," "Traditionally
  Avoided," and a "North Indian vs. South Indian view" callout. Edit the text directly.
- **Plants & trees**: edit the `PLANTS` array in `js/plants-data.js`.
- **Room checklist / remedies**: edit `js/checker-data.js` (per-room checklist items + inline remedies) or
  `js/remedies-data.js` (the standalone Remedies Library).
- **Compass / dashboard / daily tips**: all pull from the single `DIRECTIONS` array in `js/compass-data.js`.
- **Badges**: add or edit entries in `BADGE_DEFS` in `js/badges.js`, then call `maybeUnlockAndNotify("your-id")`
  wherever the achievement condition is met.
- **Quiz**: edit the `QUIZ_QUESTIONS` array in `js/quiz-data.js` — each entry is a question, 4 options, the correct
  index, and an explanation shown after answering.
- **"Ask a Question"**: change the `OWNER_EMAIL` constant near the top of the script in `ask.html` to whichever
  inbox should receive questions. It currently opens the visitor's email app with a pre-filled draft — no backend,
  no stored data. If you'd rather have submissions land in a dashboard instead of your inbox, swap that `mailto:`
  logic for a form service like Formspree (needs a free account and one endpoint URL, no other code changes).
- **Colors/branding**: the site uses Tailwind's built-in `amber` and `stone` palettes throughout. To rebrand,
  find-and-replace those class names (e.g. `amber-800` → `emerald-800`) across the HTML files.
- **Navigation**: the `ROOMS` and `TOOL_GROUPS` arrays at the top of `js/main.js` drive the header dropdowns,
  mobile menu, and footer links — add an entry there and it appears everywhere automatically.

## What's not built yet

See `roadmap.html` (or the "Roadmap" link in the footer) for a full, honest breakdown of what's live, what needs
setup, and what's intentionally out of scope for now — AR, a real astronomical Muhurat calendar, community
features, professional/multi-property accounts, payments, a native mobile app, and full regional-language
translations — along with what each would actually require to build properly.

## A note on accuracy

This content reflects commonly cited, mainstream Vastu principles intended for general cultural and informational
use. Vastu has real regional and lineage-based variation, and this guide doesn't capture every exception or school
of thought. The rule-based tools (Advisor, Plot Selection, Score Card) use simplified heuristics for orientation
purposes — they're not a substitute for an in-person consultation or professional architectural advice. See
`disclaimer.html` for the full context.
