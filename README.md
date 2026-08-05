# Vastu Guide

A room-by-room reference guide to traditional Vastu Shastra principles — covering the main entrance, living room,
kitchen, bedroom, bathroom, pooja room, staircase, study room, dining room, and balcony/garden — plus a searchable
guide to plants and trees considered auspicious or traditionally avoided near a home.

The site draws on both North and South Indian schools of thought, calling out the specific points where regional
practice tends to differ, and leads with a clear disclaimer that Vastu Shastra is a traditional belief system, not
an empirically validated science.

## Tech stack

Plain HTML, CSS, and vanilla JavaScript — no build step, no framework, no `npm install` required. Styling uses the
[Tailwind CSS Play CDN](https://tailwindcss.com/docs/installation/play-cdn) loaded directly in the browser. This
was a deliberate choice: it means the site runs identically whether you open `index.html` directly in a browser or
deploy it to a host, and there's nothing to compile or keep up to date.

```
vastu-guide/
├── index.html              Home page — room grid + intro
├── entrance.html           Main entrance
├── living-room.html
├── kitchen.html
├── bedroom.html
├── bathroom.html
├── pooja-room.html
├── staircase.html
├── study-room.html
├── dining-room.html
├── balcony-garden.html
├── plants.html             Plants & trees guide (searchable/filterable)
├── disclaimer.html         About & full disclaimer
├── css/style.css           Small amount of custom CSS on top of Tailwind
├── js/main.js              Shared header/footer/nav + disclaimer banner
├── js/plants-data.js        Plant data used by plants.html
└── vercel.json
```

## Previewing locally

No build tools needed. Any of these work:

**Option 1 — just open it**
Double-click `index.html`, or open it in your browser directly.

**Option 2 — local server (recommended, avoids any file:// quirks)**
```bash
# from inside the vastu-guide folder
python3 -m http.server 8000
# then visit http://localhost:8000
```
or, if you have Node installed:
```bash
npx serve .
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
3. Vercel will auto-detect it as a static site — no build command or output directory needed. Click **Deploy**.

**Option 2 — Vercel CLI**
```bash
npm install -g vercel   # one-time
cd vastu-guide
vercel                  # follow the prompts
vercel --prod           # deploy to production
```

Either way, there's no build step — Vercel just serves the static files as-is.

## Customizing content

- **Room pages**: each room is its own self-contained HTML file with three sections — "Traditionally Recommended,"
  "Traditionally Avoided," and a highlighted "North Indian vs. South Indian view" callout. Edit the text directly.
- **Plants & trees**: edit the `PLANTS` array in `js/plants-data.js` — the plants page renders and filters
  everything from that single array, so you can add, remove, or edit entries without touching `plants.html`.
- **Colors/branding**: the site uses Tailwind's built-in `amber` and `stone` color palettes throughout. To rebrand,
  find-and-replace those class names (e.g. `amber-800` → `emerald-800`) across the HTML files.
- **Navigation**: the room list used to build the nav menu lives in the `ROOMS` array at the top of `js/main.js` —
  add a new room there and it will automatically appear in the header dropdown, mobile menu, and footer.

## A note on accuracy

This content reflects commonly cited, mainstream Vastu principles intended for general cultural and informational
use. Vastu has real regional and lineage-based variation, and this guide doesn't capture every exception or school
of thought. See `disclaimer.html` (or the "About & Disclaimer" page on the live site) for the full context.
