# tq-chi-lander

Techqueria Chicago's link-in-bio page (https://tqchi.com/) — a single-page,
mobile-first "linktree" style site: logo, a short intro, a handful of CTA
buttons (LinkedIn, membership, donate, partner), and a list of chapter
directors. That's the whole product. No routing, no backend, no accounts.

Expect this to get a full visual revamp — the current UI is pre-AI-assisted
and will likely be rebuilt from scratch. Don't treat existing markup/CSS as
gospel; treat the *concept* (dead-simple, mobile-first, link-in-bio) as the
constraint to preserve.

## Stack

- Static HTML/CSS/JS, no framework.
- Webpack only bundles `js/app.js` (see `webpack.common.js`); styling is
  plain CSS, currently Bootstrap 5 + Font Awesome via CDN plus inline styles
  in `index.html`.
- `npm start` — dev server (webpack-dev-server, opens browser automatically)
- `npm run build` — production build to `dist/`

## Priorities when working here

- Mobile viewport is the primary target — most traffic is a phone opening a
  link from a bio/QR code. Always sanity-check mobile before desktop.
- Keep it lightweight: this should load fast and stay a single page.
- Prefer simple, direct edits over new abstractions — this is a small static
  site, not an app.

## Brand

Source of truth: [Techqueria national brand page](https://techqueria.org/about/brand/).
Where national is silent, defaults below are borrowed from The PDD (Jorge's
other project) because they already read well on mobile. **National wins on
any conflict.**

### Language

- The word is **Latine**. Not Latinx, not Latiné, not Latino/a. Applies to
  all copy, meta tags, alt text, and OG/Twitter descriptions.
- Spanish touches are on brand ("¡Hola!", "Hecho con ganas"). Keep them
  short and natural, never translated in parentheses.

### Typography

- Rubik (Google Fonts) for everything — national's only font rule. No
  second typeface; hierarchy comes from Rubik's weight range (300–900).
- Type scale (mobile first, then 768px+):
  - Display / hero word ("CHICAGO"): Rubik 900, uppercase, wide tracking,
    48px mobile / 80px desktop, line-height 1
  - H1: 28px / 42px, weight 600, line-height tight
  - H2: 24px / 36px, weight 600, line-height snug
  - Subheading: 20px / 24px, weight 500
  - Body: 16px, weight 400, line-height relaxed (1.625)
  - Label / small UI text: 15px
  - Eyebrow above a display word: 16–18px, weight 500, uppercase, widest
    tracking, 80% opacity

### Color

National palette — use these as CSS custom properties:

| Token | Hex | Role here |
|---|---|---|
| `--tq-yellow` | `#fabf67` | Primary brand color. Accents, headings on dark |
| `--flamingo` | `#ff551f` | Donate / high-energy CTA |
| `--rust` | `#9f2600` | Hover / pressed state for flamingo |
| `--amazon-green` | `#4c895c` | Partner CTA |
| `--madison` | `#2f4052` | Dark text on light surfaces |
| `--gray` | `#a3b1bf` | Muted text, borders |
| `--boston-blue` | `#3f8db9` | Links / info |
| `--fun-blue` | `#1756a9` | Secondary CTA |
| `--kabul` | `#5a4640` | Raised surface on the brown background |
| `--rebel` | `#46352f` | Page background, theme-color |
| `--black` | `#000000` | (national page has a typo here — use this) |
| `--white` | `#ffffff` | (national page has a typo here — use this) |

Rules:

- Only national palette colors. No off-palette brand blues (e.g. LinkedIn
  `#0077b5`) — use the platform icon instead to signal the destination.
- Contrast: white text on `--tq-yellow` fails (~1.6:1) — yellow buttons get
  `--rebel` or `--madison` text instead. White on `--flamingo` only passes
  at large bold sizes (24px bold+). Check every new pairing against WCAG AA.

### Logo

- Designer: Jazmin Avila. Assets come from the national brand page, PNG
  only (TODO: get SVGs from national for sharp rendering).
- Variations: Logo & Name, Logo, Name & Tagline, Name & Small Tagline,
  Circle, Acronym — each in yellow and transparent background.
- On the `--rebel` background, use the transparent version.
- Chapter lockup = national logo with "Chicago" set beneath in the display
  style. Do not redraw, recolor, stretch, or add effects to the national
  mark.

### Layout and components (PDD defaults)

- Mobile is the primary target. Minimum supported width 315px.
- Side gutters 16px mobile, 24px tablet. Content column max 600px, centered.
- Full-height sections use `100svh` (not `100vh`) so mobile browser chrome
  doesn't crop them.
- Radius: 0.5rem base; buttons and cards use 0.75rem (rounded-xl). Pill
  (fully rounded) only for single primary CTAs.
- Link buttons: full width on mobile, icon left, label left-aligned,
  trailing arrow on external links, 48px minimum tap height.
- Hover: darken one step (e.g. flamingo → rust), 200ms ease. No movement
  on hover; tap targets shouldn't jump.
- Cards: 1px border in `--gray` at low opacity, subtle surface lift from
  the background (`--kabul` on `--rebel`), shadow only on light themes.
- Animations: fade/slide in only, respect `prefers-reduced-motion`.

### Voice

Not defined yet. Until it is, keep copy short, warm, and direct, and match
the existing site's tone.
