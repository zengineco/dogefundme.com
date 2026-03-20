# DogeFundMe

**Tip · Build · Believe**

A Dogecoin tipping platform, streamer utility, community guest book, and the public funding initiative behind a real-world destination — all running as a static site on GitHub Pages. No server. No database. No monthly bill.

---

## What's in the repo

```
dogefundme/
├── index.html          ← Main site (rename from dogefundme_qr_v1.2.0.html)
├── guestbook.html      ← Guest book history page
├── logo.png            ← DogeFundMe logo (house + Ð symbol)
└── README.md           ← This file
```

That's it. Four files. The entire product.

---

## Quick deploy (5 minutes)

### 1. Create the repo

Go to [github.com/new](https://github.com/new) and create a new repository. Name it `dogefundme` or anything you like. Set it to **Public** (required for free GitHub Pages).

### 2. Add the files

Rename `dogefundme_qr_v1.2.0.html` to `index.html` before uploading. Then add all four files to the root of the repo — no subfolders.

Your repo root should look exactly like the tree above.

### 3. Enable GitHub Pages

Go to your repo → **Settings** → **Pages** → under **Source**, select **Deploy from a branch** → choose `main` → folder `/root` → **Save**.

Wait 60–90 seconds. Your site will be live at:

```
https://[your-username].github.io/dogefundme/
```

Or if you've connected a custom domain:

```
https://dogefundme.com
```

### 4. Connect your custom domain (optional)

In **Settings → Pages → Custom domain**, enter `dogefundme.com`. Then go to your DNS provider and add:

```
Type    Host    Value
A       @       185.199.108.153
A       @       185.199.109.153
A       @       185.199.110.153
A       @       185.199.111.153
CNAME   www     [your-username].github.io
```

DNS propagation takes up to 48 hours. GitHub will auto-provision an SSL certificate once it resolves.

---

## Setup checklist

### Logo

Place your logo PNG in the repo root named exactly `logo.png`. The site loads it at `./logo.png` and composites it into the center of every generated QR code. If the file is missing or fails to load, the site falls back to a gold Ð glyph automatically — nothing breaks.

### Web3Forms (guest book email notifications)

The guest book form submits to [web3forms.com](https://web3forms.com). Without a key, entries still appear live on the page and persist in the browser but you won't receive email notifications.

To activate:

1. Go to [web3forms.com](https://web3forms.com) and sign up free
2. Create a form — you'll get an access key that looks like `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`
3. Open `index.html` and find this line in the JS:

```javascript
access_key: 'YOUR_WEB3FORMS_KEY',
```

Replace `YOUR_WEB3FORMS_KEY` with your real key. Save. Push to GitHub.

That's it — every guest book submission now hits your inbox.

### Visitor counter

The counter uses [CountAPI](https://countapi.xyz) — a free, serverless hit counter. It's wired to the namespace `dogefundme.com` and key `visitors`. The counter has a hard floor of 420: if the real count is below 420 the site displays 420, giving the project a credible baseline from day one. The 421st real visitor breaks the floor and the count grows from there.

No setup required. CountAPI works automatically. If it's unreachable the site falls back to 420 silently.

---

## The site — section by section

### QR Tip Generator

Streamers and creators enter their DOGE wallet address, pick a size and color scheme, and download a branded QR code with the DogeFundMe logo centered. The QR encodes a `dogecoin:` URI — scanning it on a mobile phone opens the user's DOGE wallet app with the address pre-filled. No account, no middleman, no fee taken.

**Supported embed targets:**
- OBS browser source (copy button generates an `<img>` tag with embedded data URL)
- Twitch panels (paste the embed code directly)
- YouTube descriptions (paste the wallet address or QR link)
- Kick overlays
- X / Twitter posts
- Printed overlays, signs, merch

**Color wheel:** The QR module colors are fully customizable via an HSV color wheel. Dark and light module colors are independently adjustable. Default colors match the DogeFundMe logo: navy `#0f1f3d` and cream `#f0e8d0`.

**Sizes:** 128px (OBS overlays, small panels), 256px (standard embeds), 512px (print, merch).

**Error correction:** Level H (30% module tolerance) — the logo overlay occupies ~22% of the QR area, staying safely within tolerance.

### Such Tip

A personal tip widget pre-wired to the DogeFundMe wallet (`DU64oNpbRNRCEZdRWN1STN2KWsj7W3Ar4U`). Includes:

- Wallet address display with one-tap copy
- **Open in wallet app** button — fires a `dogecoin:` URI directly, no QR scanner needed
- Quick amount presets (10 / 50 / 100 / 420 Ð)
- Custom amount field
- Live QR that updates when the amount changes
- Session counters (address copies, link shares)

### The Destination

The public funding initiative and vision statement for a real-world community destination. Includes a Phase A–D roadmap with live phase indicator, four manifesto blocks, and a Founders Wall CTA. The wallet displayed in the Founders section is the same tip wallet — every contribution goes to the same public address.

**Phases:**

| Phase | Goal | Description |
|-------|------|-------------|
| A | $0 → $250k | Proof of demand — public wallet, live balance, top contributors |
| B | $250k → $1M | Land + legal — entity formation, location community vote |
| C | $1M → $3M | First build — boutique destination, crypto-native, Founders Wall |
| D | $3M+ | Expansion — Family Fun Center, restaurant, live events |

### Visitor Counter

A real-time hit counter powered by CountAPI. Displays a progress bar toward the 10,000 goal. Animates on page load.

### Guest Book

A hotel-lobby-themed guest book with a parchment/gold-leaf aesthetic. Visitors leave their name, an optional DOGE wallet address, and a message. Entries persist in `localStorage` so the history page (`guestbook.html`) reflects them immediately.

`guestbook.html` is a full history page with search, sort (newest/oldest), signature count, and IM Fell English serif typography for authenticity.

---

## How the QR codes work (security notes)

**What the QR encodes:**

```
dogecoin:DU64oNpbRNRCEZdRWN1STN2KWsj7W3Ar4U?amount=10
```

This is a standard `dogecoin:` URI — a payment request. It contains your wallet *address* only. Your private key never leaves your wallet app and is never involved in generating the QR.

**What happens when someone scans:**

1. If they have a DOGE wallet app installed, it opens with the address and suggested amount pre-filled. They confirm — or change the amount — before anything is sent.
2. If they don't have a wallet app, the OS shows "no app found" or redirects to the App Store.
3. Nothing is charged automatically. The sender always confirms.

**Is it safe to display your wallet address publicly?**

Yes. A wallet address is equivalent to a bank account number — others can send you money but cannot withdraw from it. The only risk is spam transactions (tiny amounts sent repeatedly), which are harmless.

**Can you link to wallets without a QR code?**

Yes. The "Open in wallet app" button fires the `dogecoin:` URI as a plain hyperlink — tapping it on mobile does the same thing as scanning the QR. You can also add this link anywhere:

```html
<a href="dogecoin:DU64oNpbRNRCEZdRWN1STN2KWsj7W3Ar4U?amount=10">Send 10 DOGE</a>
```

---

## Tech stack

| Layer | Technology | Why |
|-------|------------|-----|
| Hosting | GitHub Pages | Free, static, CDN-backed |
| QR generation | qrcode.js (cdnjs) | Pure JS, no server, error correction level H |
| Counter | CountAPI | Free serverless hit counter, no signup |
| Form submissions | Web3Forms | Free, email delivery, no server |
| Fonts | Google Fonts (Syne + Space Mono + IM Fell English) | Loaded via CDN |
| Canvas compositing | HTML5 Canvas 2D API | Logo overlay, QR rendering |
| Color picker | Custom HSV wheel | Pure Canvas, no library |
| Storage | localStorage | Guest book persistence, session counters |

Zero npm. Zero build step. Zero server. Open the HTML file locally and it works.

---

## Code standards

All JavaScript in this project follows the Zengine coding standard:

- `var` only — no `let` or `const`
- Regular `function` declarations — no arrow functions
- Every async operation wrapped in `try/catch` with named `console.error()`
- `DEV_MODE` constant at top of CONFIG BLOCK for verbose logging (set `true` to enable)
- No base64 encoding in source
- No code truncation
- Single-file delivery — everything in `index.html`, no external JS files

---

## Versioning

| Version | Description |
|---------|-------------|
| v1.0.0 | Initial build — QR generator, Shiba overlay, flat dark theme |
| v1.0.1 | Replaced Shiba with Ð serif glyph overlay |
| v1.0.2 | HSV colour wheel replaces preset theme swatches |
| v1.0.3 | Real logo PNG composite via drawImage(), Such Tip section added |
| v1.0.4 | Such Tip simplified — single card, default wallet pre-wired, custom amount |
| v1.1.0 | Full glassmorphism overhaul, IOT / Destination section, hero rewritten |
| v1.2.0 | Visitor counter (CountAPI), Guest Book (Web3Forms), guestbook.html history page |
| v1.3.0 | og/Twitter meta tags, Dogechain live fund balance, founders social proof, CountAPI localStorage fallback + exponential backoff, 18+ disclaimer, dead function cleanup |

---

## Roadmap

**v1.4.0 — Arcade**
- Off to the Races: 1 DOGE entry, fastest time wins daily pool
- Daily leaderboard with reset
- Prize pool split: 90% to winners, 10% to Destination fund
- Requires FL gaming attorney review before launch (~$300 one call)

**v1.5.0 — Streamer Dashboard**
- Save multiple QR codes (different wallets, sizes, themes)
- Usage counter per QR
- Subscription gate for premium themes ($3/month)

**v2.0.0 — Backend Migration**
- Supabase free tier — guest book real IP dedup, cross-device history sync
- Transparency page — live wallet transaction feed, phase progress
- Off to the Races live (post legal review)

---

## File delivery notes

When deploying to GitHub, the repo structure must be flat — no subfolders. Both `index.html` and `guestbook.html` must be in the root directory because `guestbook.html` links back to `./index.html` and the logo loads from `./logo.png`.

If you add the arcade or additional pages later, keep them in root as well unless you update all relative paths.

---

## Legal notes

The Destination funding initiative is structured as a community aspiration, not a securities offering. The site makes no promise of ROI, guaranteed return, or equity stake. Contributors are donating toward a shared community goal. This framing should be preserved in all copy updates.

The guest book operates on an honor system with no IP enforcement in v1. One signature per person is requested but not technically enforced. Upgrade path: Supabase free tier with IP hashing (v2.0.0 roadmap).

---

## Contact

Built by Vincent Gonzalez — [Zengine™](https://zengine.site)

Site: [dogefundme.com](https://dogefundme.com)

Wallet: `DU64oNpbRNRCEZdRWN1STN2KWsj7W3Ar4U`

---

*© 2026 Zengine™ — www.zengine.site*
