# Boost SE Demo

An offline, responsive demo of the [Boost](https://boostapp.io) food ordering app for Sales Engineering demos. It is a brand reskin of the Thrive SE Demo (`..\Thrive.io Copy`) — identical venues, menus, prices, photos and flows; only brand tokens, typography, wordmark, favicon and copy differ. Runs entirely in the browser from local files; no server, no build step, no frameworks.

## Quick Start

1. Open `index.html` in Chrome.
2. Resize the window: below 900px it is the phone app (bottom nav, Scan & Pay FAB); at 900px and up it becomes the desktop site (left rail, centered 1200px column) like boostapp.io in a browser.

Fonts load from Google Fonts on first load (cached afterward); everything else is fully offline.

> **Demonstration only.** Menus, prices, the user account and payments are simulated. This site is not connected to the live Boost service and is not an official Boost or Compass Group product.

## Structure

```
index.html                     Single-file vanilla JS SPA (all HTML/CSS/JS)
robots.txt                     Disallow all crawlers (page also carries noindex + a CSP meta)
images/                        Menu and venue photos, boost-icon.png (app icon) and favicon.png
reference/scripts/             Image re-download helpers (from Thrive)
reference/BOOST_PROJECT_CONTEXT.md   Working notes for continuing in Claude
```

## Wordmark

`.wordmark` is a lockup of the official Boost app icon (`images/boost-icon.png`, from `~\Claude\boost icon\boost_icon_192.png`) beside "Boost" set in Nunito Sans 800. It appears in the home header (`.wordmark.light.sm`, white text, 24px) and the site-selector sheet (teal text, 28px). If an official horizontal logo is supplied later, replace the `<span class="wordmark">` contents with a single `<img>`. `images/favicon.png` is the same icon resized from the 2401px master.

## Design Tokens

Source: Boost brand guidelines "The Colors" page for brand colors; surfaces and text sampled from boostapp.io (Sept 2026). Variable names were kept from Thrive so no downstream CSS rule had to change; comments in `:root` name each color.

| Token | Value | Source | Used for |
|-------|-------|--------|----------|
| `--green` | `#008078` | Guide — Primary Teal 1, Boost Green | Buttons, links, active nav, cart bar (live site renders `#006A63`; guide value chosen) |
| `--green-2` | `#004D47` | Guide — Queensland Teal | Section labels, dark accents |
| `--green-3` | `#69B8AD` | Guide — Primary Teal 2 | Light accents |
| `--green-4` | `#C0DCDA` | Guide — Primary Teal 3, Bkground Green | Tinted highlights |
| `--green-5` | `#E3ECEA` | derived | Image placeholders |
| `--page-bg` | `#FFFFFF` | live site | Content column |
| `--outer` | `#F2F2F2` | live site | Canvas behind the column on desktop |
| `--surface` | `#EFF1ED` | live site | Sign-in card, category tab strip, hover |
| `--rail` | `#F8FAF6` | live site | Desktop side rail |
| `--text` | `#191C1A` | live site | Body text and headings |
| `--muted` | `#5F6B68` | derived | Secondary text |
| `--border` | `#E1E5E2` | derived | Dividers |
| `--rust` | `#D90057` | Guide — Error, Boost Pink | Cart count badge (as on live site), errors |
| `--honey` / `--accent` | `#D37A00` / `#F5A622` | Guide — Secondary 1 / 2 | Reserved |
| Promo tile | `#013934` / `#BACE31` | live site "Unlock More Rewards" tile | What's new card only |
| Fonts | Nunito Sans | — | Closest free match to the guide's Avenir-style type |

## Responsive Layout

Phone-first. One `@media (min-width:900px)` block turns the app into the desktop site: `#side-rail` (Home / Orders / Support / Help) replaces `#bottom-nav` and the FAB; every `.screen` becomes a flex column padded to a centered 1200px white column on the `--outer` grey canvas; the hero grows to 480px with a left-aligned 40px greeting; venue cards go to a 3-column grid; the green "Ready to place your order? / Continue" bar (`#cart-bar`, shown by `updateBadges()` whenever the basket has items on a browse screen) spans the column. The item screen keeps its own internal flex/scroll layout (`#screen-item::after{display:none}`).

## Changes vs. Thrive

- `:root` → Boost palette; hardcoded greys (`#c8ddc8 #deeede #e8f0e8 #aaa #0a1f1e #E5EFE9 #EFF1EE #F5F5F5`, `rgba(17,57,54,…)`) → teal equivalents
- DM Sans / Inter → Nunito Sans
- Cart badge → Boost Pink on white (matches live site)
- `<title>` → "Boost — SE Demo Site"; site sheet subtitle → "Boost Food Ordering — Sales Engineering Demo"
- Wordmark slot added (header + site sheet); `.site-link` reset (`background:none;border:none;padding:0`)
- Home header switched from `position:sticky` inside the clipped hero to `.hdr.over` (absolute overlay) — in the Thrive source it was pushed below the 270px hero and never visible
- `images/favicon.png` → official Boost app icon (180px); `images/boost-icon.png` added for the wordmark lockup
- Home gains the live site's sign-in card ("Come and enjoy the full Boost experience") and a CSS-only "What's new" rewards promo tile; both show toasts
- New: desktop side rail, `#cart-bar`, responsive breakpoint at 900px (see Responsive Layout); `.primary-btn` radius 50px → 10px to match live buttons
- Thrive brand assets, source images and QR code under `reference/` not copied

## Open Items

- Optional: official horizontal Boost logo to replace the icon + text lockup
- Confirm official typeface (brand PDF is image-only; Nunito Sans is a stand-in)
- Optional: Boost-specific hero photography (`images/img_hero.png` is the neutral chef photo from Thrive)
- Beverage/side modifiers, receipt screen, loyalty, real Scan & Pay, cart quantity steppers (inherited gaps)

## Publishing Checklist

- No real people or clients: the account tab shows a fictional user (`Alex Demo`), site names are fictional. Keep it that way.
- No secrets: the app is fully static and must stay that way. Never add live Boost / CentricOS endpoints or keys to this file; a real integration belongs behind a server.
- `reference/` is git-ignored (brand PDF, working notes with local paths, image scripts) — don't force-add it.
- `<meta http-equiv="Content-Security-Policy">` limits the page to itself plus Google Fonts (`connect-src 'none'`, `object-src 'none'`). Clickjacking protection (`frame-ancestors` / `X-Frame-Options`) can only be set as an HTTP header by the host, not in the page. If you add an external resource, extend the policy deliberately.
- `robots.txt` + `noindex` keep the demo out of search if it is hosted.
- On GitHub: keep the repo private unless there's a reason not to; enable secret scanning and push protection (Settings → Code security).
- Optional: self-host the Nunito Sans woff2 files to remove the only third-party request.

## Image Credits

Food and venue photography from [Unsplash](https://unsplash.com) (Unsplash License), committed locally.
