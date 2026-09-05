# Welcome Mart — Website

A production-ready, single-file business website for **Welcome Mart**, a grocery store in
Siddhpur, Gujarat.

> `index.html` — that is the whole site. Open it in a browser, or drop it on any static host
> (Netlify, Vercel, GitHub Pages, Hostinger, cPanel, S3). No build step, no dependencies,
> no framework, no trackers.

---

## What's inside

| Section | Anchor | Notes |
| --- | --- | --- |
| Sticky header + mobile drawer | — | Logo (fallback tile if it ever fails to load), 6 nav links, Get Directions, animated hamburger |
| Hero | `#top` | Ribbon backdrop, headline with a folded red tag, two CTAs, Google rating proof, and a **3D showpiece card** that tilts toward the pointer |
| Trust / quick-info strip | — | Google rating · review count · hours · location |
| About | `#about` | Store intro, four value points, "Visit Welcome Mart" CTA |
| Product categories | `#categories` | 8 cards (Grocery & Food → Daily Essentials), each with its own `image` slot; clicking one filters the products |
| Featured products | `#products` | 12 tiles with a working category filter, a photo slot each, "Available in Store" label, **no prices** |
| Why shop here | — | 4 feature cards on deep royal blue |
| Google reviews | `#reviews` | 4.1 / 5 · 20 reviews · stars rendered to 4.1 — **no invented quotes** |
| Location | `#location` | Full address, copy-address button, **live Google map** in an iframe, Get Directions (the store's own Maps link), plus a wide neighbourhood map under the hours row |
| Store hours | `#hours` | Mon–Sun 9:00 AM – 8:00 PM, today highlighted, live **Open now / Closed** pill (Asia/Kolkata) |
| Gallery | `#gallery` | 6 frames with a keyboard-accessible lightbox — each frame is a real `<img>` |
| Before you visit | `#faq` | 5 native `<details>` accordions (also published as FAQ structured data) |
| Final CTA | `#contact` | Get Directions · Explore Categories |
| Footer | — | Brand, quick links, store information, opening hours |
| Mobile action bar | — | Fixed **Directions / Store** bar, phones only, never blocks content |

**Palette** — taken from the client's poster: deep royal blue as the primary colour, poster
red as the accent, white backgrounds with navy-tinted charcoal text. Blue carries the brand
surfaces (buttons, header, panels), red is reserved for emphasis (headline tag, ratings,
highlights), and the green you see once — the *Open now* dot — is deliberately semantic.

**Verified business data only** — name, category, address, 4.1★ / 20 reviews, daily
9:00 AM – 8:00 PM. Everything else on the page is either generic website copy or clearly
labelled placeholder content.

---

## Editing content

Everything a client or developer is likely to change sits in one clearly-labelled config block
under `WELCOME MART — SITE RUNTIME`, near the bottom of `index.html`. Seven sections —
`business`, `maps`, `images`, `nav`, `categories`, `products`, `gallery` — are assembled into a
single `WM_DATA` object (also available as `window.WM_DATA` in the browser console):

```js
var business = {
  name: 'Welcome Mart',
  address: { full: 'F 9, Zavari Complex, 12, Road, ...' },
  rating: 4.1,
  reviewCount: 20,
  hours: { openHour: 9, closeHour: 20, label: '9:00 AM – 8:00 PM', timeZone: 'Asia/Kolkata' },

  /* Not supplied yet — leave null and the matching UI stays hidden. */
  phone: null, whatsapp: null, email: null, googleBusinessUrl: null, gstin: null
};
```

### The Directions link

Every **Get Directions** control — header, hero, about, location panel, hours card, final CTA
and the mobile action bar — opens the store's own Google Maps link (7 links, one source of
truth):

```js
var directionsShortLink = 'https://maps.app.goo.gl/rPTW35E5SdzxgM726';   // supplied by Welcome Mart
```

The same URL is written into the static `href`s as well, so the buttons are already correct
before any JavaScript runs. If you ever clear that value, the site automatically falls back to
a directions link built from the verified address
(`https://www.google.com/maps/dir/?api=1&destination=…`) — no other edit needed, and the
address-based *Search on Google Maps* link next to the map stays as it is.

### The hero photograph

The store's own picture is the front face of the 3-D hero card — one `<img>` inside
`.hero__card`, above the photo slot and the sheen, so nothing is painted over it except the
small corner chip:

```html
<img class="hero__photo" src="https://i.ibb.co/Qjj85ZsZ/Screenshot-20260820-222726.jpg" alt="Welcome Mart" />
```

To change the shot, edit that one `src` (or set `images.hero` in the config and delete the
`<img>` — both paths render the same way). Behaviour worth knowing:

- a **wide** image is cropped edge-to-edge (`object-fit: cover`); a **tall or square** source —
  a phone screenshot, for example — is shown whole, with the card's own gradient as the
  letterbox plate, because JS measures it and adds `.is-portrait`
- `alt` currently names the store; rewrite it to describe what the photo actually shows
- if the file ever fails to load (or stalls past 3 s), `.is-missing` hides it and the photo
  slot underneath shows instead — the hero is never left half-empty
- hover is the only extra motion: the card straightens and rises, the coloured slabs spread,
  the chips lift, the pan shifts a few percent, and the idle float pauses. Everything is
  disabled under `prefers-reduced-motion`, and the image itself has no overlay effects.

### The logo and the favicon

The business logo is used in three places — the header brand link, the footer brand lockup
and the browser tab / home-screen icon — all pointing at the same hosted file:

```html
<link rel="icon" type="image/png" href="https://i.ibb.co/dsr6wZCh/a-And-also-1-1-in-this.png" />
<img class="brand__logo" src="https://i.ibb.co/dsr6wZCh/a-And-also-1-1-in-this.png" alt="" />
```

Search the file for `i.ibb.co` to change it (four occurrences: two links, two images). The
logo is height-locked with `object-fit: contain`, so any aspect ratio lands on the same
baseline without distortion, and it never breaks the layout: if the image fails to load — or
the request stalls for more than 2.5 s — the generated basket tile is put back automatically.

The mark itself is a rounded white plate with a soft drop shadow (`.brand__logo`), so a
transparent PNG or a square lockup never reads as a bare rectangle against the header. Change
`border-radius` for a circle (`50%`) or a harder edge, and `box-shadow` for more or less lift;
`.brand:hover .brand__logo` holds the hover state.

For production, self-hosting is the better move (one less third-party dependency):

```text
assets/logo.png     ← save the file here, then replace the URL in those four places
```

On the dark footer, `.footer__logo .brand__logo` keeps the same rounded plate and swaps the
soft shadow for a deeper one plus a hairline ring; adjust `padding` there. Two other knobs are
worth knowing: the desktop width of the mark is capped (`max-inline-size`) so a long
horizontal lockup can never push the menu out, and if your logo already contains the words
"Welcome Mart", hide the text beside it with `.brand__text { display: none; }`.

### Replace the demo images

Every image on the page is a real `<img>` — there is no illustration layer left to unwind.
Until the shop supplies photographs, each slot paints a **demo plate**: a small inline SVG
(labelled *DEMO PHOTO — REPLACE WITH YOURS*, in the brand colours) carried inside the `src`
itself, so it needs no network request, can never 404, and can't be mistaken for a picture of
the store.

Three ways to put a photograph in — start with the first, it is the one that covers the
"Browse the store" and "Popular picks" grids:

**1. Set `image` on the item itself.** Every category card, product card and gallery frame has
its own `image` line right beside its name in the config lists (`categories` = section 5,
`products` = section 6, `gallery` = section 7), and that value *is* the card's `<img src>`:

```js
var products = [
  { name: 'Rice', image: 'images/rice.jpg', category: 'grocery-food', icon: 'i-wheat', note: 'Pantry staple' },
  { name: 'Wheat Flour', image: '', category: 'grocery-food', icon: 'i-wheat', note: 'Pantry staple' },
  //                        ↑ left empty, this card keeps its demo plate
];
```

Nothing else needs touching: the card renders exactly one image tag, and the comments next to
`#cat-grid` and `#prod-grid` in the markup show it, so you can see what a path turns into:

```html
<img src="images/rice.jpg" alt="Rice available in store at Welcome Mart" loading="lazy" decoding="async" />
```

Categories use the same field (`image` on the category entry fills that card's photo strip in
the 16:9 frame), and so do the six gallery frames — the gallery image also feeds the lightbox,
so one path covers both views.

**2. Edit the `<img>` in the markup** for the two static frames. The hero and about frames carry
their plate directly, tagged so they are easy to find — search for `data-img="hero"` and
`data-img="about"`:

```html
<img class="hero__art" data-img="hero" alt="Welcome Mart store front" src="images/store-front.jpg" />
```

**3. Fill the `images.*` maps** when you would rather do a whole section in one place — keys
are the category/gallery `id`, or the product `name`:

```js
var images = {
  hero:  'images/store-front.jpg',   // 3-D hero card
  about: 'images/inside.jpg',        // about frame
  categories: { 'grocery-food': 'images/grocery.jpg' },
  products:   { 'Rice': 'images/rice.jpg' },
  gallery:    { front: 'images/front.jpg' }
};
```

**Precedence: the item's own `image` → the matching key in `images.categories` /
`images.products` / `images.gallery` → demo plate.** For the two markup frames, `applyImages()`
re-points the `src` (and the `alt` from `images.alt`) once a value exists. Behaviour worth
knowing:

- `alt` is written for you as `Demo image — …(replace with a real photograph)` while a slot is
  a plate, and becomes a plain description once a photo is set — honest text either way
- **a wrong path can't leave a broken-image icon**: the `error` handler paints that slot's demo
  plate back over the failed URL (so a typo costs nothing but the picture)
- plates are vector, so they stay sharp at any size; every frame already sets its own
  `aspect-ratio` in CSS, so nothing shifts while images decode
- shoot landscape where you can — frames are 3D-hero 5/4, about 4/3, category 16/9, product
  4/3, gallery 4/3 (wide tiles 16/9) — and 1200 px on the long side is plenty
- name the files after the item as you export them (`rice.jpg`, `grocery.jpg`); the path is the
  only thing you paste, and product photos are reused nowhere else, so one file per product is
  enough
- images below the fold are `loading="lazy"` and `decoding="async"` automatically

Nothing else in the page needs touching when you add photographs; delete no markup.

### The live Google maps

Two real Google map frames ship in the location section — no API key, no coordinates:

| Where | What it shows | Config knob |
| --- | --- | --- |
| In the "Visit Welcome Mart" panel, beside the address and hours | the storefront pin area, `z=17` | `maps.embedUrl` |
| Full width, below the address / hours / panel row | the same address, wider neighbourhood view, `z=15` | `maps.areaEmbedUrl` |

Both `src` URLs are written straight into the markup, built from the verified street address
(`https://www.google.com/maps?q=Welcome%20Mart%2C%20F%209%2C%20Zavari%20Complex…&output=embed`),
so the map is already live in the HTML — it works with JavaScript disabled, and the browser
starts it lazily as the section scrolls near.

To use Google's own embed instead (the one with the exact business pin): in Google Maps →
search the store → **Share → Embed a map → Copy HTML**, then paste only the `src` URL into
`maps.embedUrl` (and `maps.areaEmbedUrl` if you want the wide frame to match). `applyMap()`
re-points both frames from those two values and leaves a frame alone when the URL already
matches, so nothing loads twice.

Under the panel map sits a small caption row: *Live Google map — positioned from the verified
address*, an **Open this area in Google Maps** link and the **Get Directions** button. It lives
below the frame rather than on top of it, so nothing swallows the map's own drag and zoom
gestures. On paper the frames are hidden (`@media print`) and the address card carries the
facts.

If Google is ever unreachable for a visitor, the frame simply stays an empty card — the caption
row below it carries the address, the area-search link and Get Directions, so the map is an
enhancement and never the only way to find the store.

### Show a "View Google Reviews" button

The button is hidden until a real listing URL exists — no fake link:

```js
business.googleBusinessUrl = 'https://www.google.com/maps/place/…';
```

### Re-brand the whole site

Colours, radii, shadows, spacing and type scale are CSS custom properties in one `:root`
block at the top of the file — `--wm-blue-*` (the primary ramp), `--wm-red-*` (the accent
ramp), `--wm-ink-*` (text), plus lines, surfaces and shadows. The components read the
semantic aliases (`--color-primary`, `--color-accent`, `--color-text`, …), so re-pointing
those ~10 alias lines re-skins every section, card, button and icon at once.
The demo plates are the only exception: they are drawn inside `demoPlate()` in the script, so
re-colour three hard-coded values there (`#f6f8fe`/`#dbe6fb` background, `#1b2a7a` ink,
`#e4002b` tick) if you want the placeholders to follow a new palette. Real photographs make
the question moot.

---

## Technical notes

- **Responsive:** mobile-first, tested against 320 / 360 / 390 / 430 / 768 / 1024 / 1280 /
  1440 / 1920 widths — dedicated layouts for phones, not a shrunk desktop.
- **Accessibility:** skip link, landmarks, ordered headings, labelled controls, focus-visible
  rings, focus trap + `Esc` on the drawer and lightbox, `aria-live` filter status,
  44px tap targets, and a complete `prefers-reduced-motion` bypass.
- **SEO:** title, meta description, Open Graph, Twitter card, `GroceryStore` JSON-LD
  (address, 9–8 hours, 4.1/20 rating) and `FAQPage` JSON-LD — verified fields only.
- **Motion:** the hero's 3D card keeps its designed angle in CSS; JavaScript only adds the
  pointer parallax on fine pointers, and `prefers-reduced-motion` removes the float, the
  ribbons' drift, the reveals and the tilt.
- **Performance:** the shipped page makes no image requests (demo plates are inline data
  URIs); each real photograph you add costs exactly one request. Both map iframes are
  `loading="lazy"`, so they only download when the location section approaches. No JS bundles,
  one IntersectionObserver for scroll reveals, `requestAnimationFrame`-throttled scroll handler.
- **Graceful degradation:** content renders without JavaScript — including the demo photographs
  and both live map frames; the drawer, filters, lightbox and live status are progressive
  enhancements (`<noscript>` fallbacks list categories and products as plain links).

## Checklist before going live

- [ ] Click `https://maps.app.goo.gl/rPTW35E5SdzxgM726` once to confirm it lands on the right
      Welcome Mart pin (it is used by all seven Directions buttons and `hasMap` in the schema)
- [ ] Photographs: storefront, aisle, counter, and the products you actually stock — every demo
      plate is a labelled `<img>`, so replace the `src` (or fill `images.*`) and the page is real
- [ ] Load the location section on a phone: both map frames should show Welcome Mart's address,
      pan and zoom, and never sit behind a cookie/consent wall
- [ ] Google Business Profile URL (enables the "View Google Reviews" button)
- [ ] Optional: paste Google's own embed `src` into `maps.embedUrl` / `maps.areaEmbedUrl`
      (replaces the address-built map frames)
- [ ] Confirm the hours and the review count are still current
- [ ] Only if the owner approves: phone / WhatsApp / delivery information
- [ ] Move the logo from `i.ibb.co` to `assets/logo.png` and update the four references
- [ ] Swap the logo for a proper 1200 × 630 share image in `og:image` / `twitter:image`
      (the logo stands in for those today)
