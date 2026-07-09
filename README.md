# Gweru Stars Awards — Public Voting Site (Frontend)

Static HTML + compiled Tailwind + vanilla JS. No build step needed to deploy —
upload `index.html`, `css/`, and `js/` to any static host (Vercel, Netlify,
Cloudflare Pages, or plain cPanel hosting) and it works.

## Files

| Path | What it is |
|---|---|
| `index.html` | Page shell — fonts, meta/social tags, mounts the app |
| `css/styles.css` | **Compiled** Tailwind (~20 KB). Deploy this, don't touch it by hand |
| `js/data.js` | All 51 categories in 9 sections, event details, demo-mode flag |
| `js/app.js` | Routing, rendering, countdown, vote flow |
| `src/input.css` + `tailwind.config.js` | Source for the CSS — only needed if you restyle |
| `smoke-test.js` | jsdom test suite (12 checks) — `npm i && node smoke-test.js` |

## The two edits you'll actually make

**1. Real nominees.** In `js/data.js`, set `DEMO_MODE = false`, then give each
category its nominees. Two options:
- Quick: hardcode them in `buildProgramme()` per category.
- Proper (recommended once the backend exists): fetch them from the API and
  build `PROGRAMME` from the response — the render layer doesn't care where
  the data came from.

Until real nominees exist for a category, an empty `nominees` array shows a
"Nominees announced soon" plaque automatically.

**2. Backend wiring.** One function: `castVote()` in `js/app.js`. The comment
block above it spells out the exact fetch call and which status codes map to
which UI states. Success, duplicate-vote (409), and network-failure handling
already exist around it — nothing else needs to change.

Note: the current localStorage "one vote" behaviour is a UI convenience only.
Anyone can clear their browser storage — real one-vote-per-IP enforcement
happens server-side (the Supabase schema from the earlier build handles this
with a DB-level unique constraint).

## Design system

- **Palette:** lacquer black `#0A0A0F`, velvet `#14101C`, trophy gold `#E9B84E`,
  champagne text `#F6E9CF`, smoke `#9A94A6`, crimson `#C13045` (reserved for
  live/vote states only)
- **Type:** Fraunces (display) + Archivo (body), loaded from Google Fonts
- **Signature:** rotating spotlight beams in the hero; gold seal stamp on a
  cast vote; the programme reads as nine Roman-numeral "acts"
- Honours `prefers-reduced-motion`, keyboard focus is always visible,
  works down to 320 px wide

## Restyling

```bash
npm install
npx tailwindcss -i src/input.css -o css/styles.css --minify
```

## Honorary awards

Lifetime Achievement, Excellence Award, and Gweru Star of the Year are marked
`votable: false` in the data — they render as plaques with "announced on the
night" and have no vote route. People's Choice Award is public voting per the
poster, so it behaves like every other votable category.
