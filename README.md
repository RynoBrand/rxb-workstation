# RXB Workstation — public site

**Generated. Do not edit these files by hand.** Everything here is rebuilt by:

```
npm run build:website
```

which wipes and recreates this folder. Edit the sources instead:

| Page | Source |
| --- | --- |
| `/` | `scripts/build-website.mjs` — `HOME_BODY` |
| `/manual/` | `RXB_Workstation_User_Manual/RXB_Workstation_User_Manual_CV2.6.MD` + `Images/` |
| `/support/` | `scripts/build-website.mjs` — `SUPPORT_BODY` |
| `/eula/` | `assets/legal/EULA.txt` |
| `/privacy/` | `assets/legal/PRIVACY_POLICY.txt` |
| `/licenses/` | `THIRD_PARTY_LICENSES.txt` |

The legal pages are generated from the same files the installer ships via
`extraResources`, so the hosted copy cannot drift from the installed one. That
is the whole reason this is a generator and not a hand-written site. Re-run the
build after changing any source above — nothing regenerates on its own.

## Steam store link

The homepage hero has a single **Buy on Steam** button (`.steam-buy`): the
Steam logo on black on the left, "Buy on Steam" on red on the right. It links to
`https://store.steampowered.com/app/5157640/RXB_Workstation/`. Every page's
footer has a "Steam store" link and Valve's trademark attribution line, and
the Support page's Refunds section links to Steam's refund process.

- A "Get RXB Workstation" panel lower on the homepage repeats the Buy on Steam
  button, and the kicker line reads "Available now on Steam".
- The stylesheet link carries a content hash (`site.css?v=<hash>`), so returning
  visitors get new styles straight away. Without it, a cached stylesheet showed
  the badge unframed after the first Steam update.
- The URL comes from one constant, `STEAM_URL` in `scripts/build-website.mjs`.
- The logo is copied at build time from
  `brand/steam/official/steam_logo_lockup_white.svg` to
  `assets/img/steam-logo-white.svg`. It is Valve's own vector artwork.
- Valve's usage rules (logo stands alone, 50 px minimum height, clear space,
  attribution) are in `brand/steam/README.md`. Read them before changing the
  badge.

## App Store link (iPad, added 24 Nov 2026)

The hero's store row (`.cta-row--stores`) holds Apple's black **Download on
the App Store** badge (`.appstore-badge`), first, then the Buy on Steam button.
Both are 64 px tall, at the owner's request, and the gap between them is 16 px,
Apple's clear space at that size. It links to
`https://apps.apple.com/app/id6834072824`.

- **64 px puts the Steam logo at 41 px, below Valve's 50 px minimum.** The
  owner chose that on 24 Nov 2026; it is recorded, with how to undo it, in
  `brand/steam/README.md` ("Second known deviation"). Change both heights
  together, or the pair stops matching.

- The kicker reads "Windows and iPad · ... · Available now on Steam and the
  App Store". `iPad` and `App Store` sit in `.tm` spans, so the upper-case
  kicker does not print them in capitals.
- The "Get RXB Workstation" panel has two text buttons, "Buy for Windows on
  Steam" and "Buy for iPad on the App Store". It does not repeat the badge,
  because Apple allows one badge per layout. The panel also says what the iPad
  leaves out (Art-Net lighting, MIDI controllers).
- Every page's footer has an "App Store" link and Apple's credit lines.
- The Support page lists both operating systems (Windows 10/11 64-bit on
  Steam, iPadOS 16 or later on the App Store), and its Refunds section sends
  App Store buyers to `reportaproblem.apple.com`.
- The URL comes from one constant, `APP_STORE_URL` in
  `scripts/build-website.mjs`.
- The badge is copied at build time from
  `brand/apple/official/Download_on_the_App_Store_Badge_US-UK_RGB_blk.svg` to
  `assets/img/app-store-badge-black.svg`. It is Apple's own artwork, unaltered.
- Apple's usage rules (black badge when other stores appear, App Store first,
  one badge per layout, 40 px minimum, clear space, credit lines, how to write
  `iPad`) are in `brand/apple/README.md`. Read them before changing the badge.

## Publishing to GitHub Pages

1. Push to a repository.
2. **Settings → Pages → Build and deployment → Deploy from a branch.**
3. GitHub Pages can only serve from the repository **root** or from `/docs`.
   It cannot serve an arbitrary subfolder, so either publish a repo whose root
   *is* this folder's contents, or copy this output to `docs/`.
4. Leave `.nojekyll` in place. Without it Pages runs Jekyll, which silently
   drops files and folders beginning with an underscore.

All asset paths are **relative**, so the same build works unchanged at a project
site (`user.github.io/repo/`), a user site, or a custom domain. Do not switch to
root-relative (`/assets/…`) paths — that breaks the project-site case, which is
how this is hosted first.

## Custom domain

To serve at `globalxbrand.com/rxb/`, a project site published at `/rxb/` is the
closest match to the planned URLs. Add a `CNAME` file containing the bare domain
and point DNS at GitHub. No HTML changes are needed.

## Before linking these URLs from a store listing

Steam and the Microsoft Store both reject broken links at review, and each
rejection costs days. Confirm every page loads at its **final public URL**
before pasting it into a store field:

```
/eula/      → Steam "EULA" / MS Store "Additional license terms"
/privacy/   → Steam + MS Store privacy policy URL (required)
/support/   → Steam support URL (required)
/manual/    → Steam "Online Manual" URL
/licenses/  → third-party notices
```

The EULA is ~9,400 characters, which is why it must be a hosted URL: the
Microsoft Store's licence-terms field caps at 10,000 characters and the text
would have to be pasted otherwise.
