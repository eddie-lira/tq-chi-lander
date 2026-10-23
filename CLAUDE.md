# tq-chi-lander

Techqueria Chicago's link-in-bio page (https://tqchi.com/) — a single-page,
mobile-first "linktree" style site: a photo header, a short intro, and a
stack of CTA buttons (LinkedIn, Slack, donate, "what is Techqueria"). That's
the whole product. No routing, no backend, no accounts.

The visual design went through heavy iteration directly with Jorge (colors,
type scale, button styling, a photo hero) and the sections below now
describe the *current, settled* design — not a starting-point default. The
Chapter Directors section and a "Partner With Us" CTA existed earlier and
were deliberately removed (directors section is slated for a redo later,
partner CTA just didn't make the cut). Don't resurrect either without
asking.

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
- As implemented (mobile first, then 768px+):
  - Chapter title ("CHICAGO CHAPTER"): Rubik 700, uppercase, 0.06em
    tracking, 32px mobile / 48px desktop, line-height ~1 — scaled down
    from an earlier 900-weight/48–80px pass that read as too big/heavy
  - Greeting ("¡Hola, Chicago!"): italic, weight 700, 28px mobile / 36px
    desktop, white (it sits over the photo hero)
  - Intro/subheading copy: 20px mobile / 24px desktop, weight 500
  - CTA subtext (under each button): 14.4px (0.9rem), `--gray`, sentence
    case — **not** uppercase/eyebrow-style. An all-caps eyebrow treatment
    was tried above the buttons and dropped for feeling "shouty"; this
    sentence-case blurb-below-the-button pattern is the current standard
    for any button annotation.

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
| `--rebel` | `#46352f` | theme-color meta tag; dark scrim base over the photo hero |
| `--black` | `#000000` | (national page has a typo here — use this) |
| `--white` | `#ffffff` | (national page has a typo here — use this) |

As implemented: the page background is plain **white**, not `--rebel` — a
dark/gradient full-page background was tried for a while (several gradient
directions, see git history if curious) and ultimately dropped in favor of
white with a photo hero up top. Only the header (`.tq-hero`) uses dark
tones, as a scrim over `img/team-photo.jpg`.

Rules:

- Only national palette colors in general — **with one deliberate,
  documented exception**: the LinkedIn icon uses real LinkedIn blue
  (`#0077b5`) for brand recognition. This applies to that icon only, never
  to a button fill or body text. No other off-palette colors.
- Contrast: white text on `--tq-yellow` fails (~1.6:1) — yellow buttons get
  `--rebel` or `--madison` text instead. White on `--flamingo` only passes
  at large bold sizes (24px bold+). Check every new pairing against WCAG AA.
- Per-button icon accent colors currently in use: LinkedIn icon = real
  LinkedIn blue (exception above), Slack icon = `--tq-yellow`, Donate icon
  = `--amazon-green`. Button label text itself stays uniformly `--madison`
  regardless of which button — only the icon carries color.

### Logo

- Designer: Jazmin Avila. Assets come from the national brand page, PNG
  only (TODO: get SVGs from national for sharp rendering).
- Variations: Logo & Name, Logo, Name & Tagline, Name & Small Tagline,
  Circle, Acronym — each in yellow and transparent background.
- On the `--rebel`/photo-scrim hero, use the transparent version
  (`img/tq-logo-2.png`).
- The square "TQ" mark (`icon.png`) doubles as a small inline icon
  elsewhere (e.g. the "What is Techqueria?" button) — it's the only asset
  with a usable square aspect ratio for that.
- Chapter lockup = national logo with "Chicago Chapter" set beneath in the
  chapter-title style. Do not redraw, recolor, stretch, or add effects to
  the national mark.
- `img/og-card.png` is a generated 1200×630 social-share image (the square
  icon padded onto a `--rebel`-colored canvas via `sips`) — regenerate it
  the same way if `icon.png` ever changes.

### Layout and components (as implemented)

- Mobile is the primary target. Minimum supported width 315px.
- Photo hero (`.tq-hero`): `img/team-photo.jpg` with a dark linear-gradient
  scrim (`rgba(36,26,22,.55)` → `rgba(36,26,22,.8)`) for text legibility.
  Gets a `min-height: 520px` + vertical centering at 800px+ viewports —
  without it, `background-size: cover` crops out a whole row of people on
  wide screens.
- Side gutters are intentionally tight — content mostly runs close to the
  container edge rather than the national-default 16px/24px gutters.
- Full-height sections use `100svh` (not `100vh`) so mobile browser chrome
  doesn't crop them. (The footer is **no longer** pinned to the viewport
  bottom via `margin-top: auto` — that looked fine when content filled the
  screen, but left a large empty gap once the page got more compact. It
  just follows content now with a normal top margin.)
- Buttons (`.btn-tq`): full width, `border-radius: 1rem`, icon+label
  **centered** as a group (not left-aligned — that was tried and reverted),
  white fill, 1px `rgba(47,64,82,.18)` border, a visible drop shadow (`0
  4px 10px rgba(47,64,82,.16)`) so they read as raised cards against the
  white page background, 48px minimum tap height. No trailing-arrow
  convention — icon is always leading.
- Button label text is always `--madison`; see the Color section above for
  the icon-accent-color convention.
- CTA subtext sits *below* its button (`.tq-note`, see Typography), not
  above it as an eyebrow.
- Hover: darken one step (e.g. flamingo → rust) or a light fill tint, 200ms
  ease. No movement on hover; tap targets shouldn't jump.
- Animations: fade/slide in only, respect `prefers-reduced-motion`.

### Voice

Not defined yet. Until it is, keep copy short, warm, and direct, and match
the existing site's tone.

## Known gaps to track (no implementation yet)

- **Campaign content has no expiry.** The Donate button's subtext ("Help us
  plan ahead for Techqueria 2027") and its `?campaign=` link param are
  tied to whatever campaign is currently active (e.g. a Q4 fundraising
  push). Nothing on this page knows when a campaign ends. Eventually we
  need some notion of an active campaign having a start/end date, and
  falling back to different (probably more generic) copy/link once it
  expires, instead of silently showing stale campaign messaging forever.
  Flagged by Jorge 2026-10-23 — explicitly deferred, not to be designed or
  built speculatively until he asks for it.
