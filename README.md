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
| Sticky header + mobile drawer | — | Logo, 6 nav links, Get Directions, animated hamburger menu |
| Hero | `#top` | Badge, headline, supporting copy, two CTAs, Google rating proof, grocery artwork |
| Trust / quick-info strip | — | Google rating · review count · hours · location |
| About | `#about` | Store intro, four value points, "Visit Welcome Mart" CTA |
| Product categories | `#categories` | 8 cards (Grocery & Food → Daily Essentials); clicking one filters the products |
| Featured products | `#products` | 12 tiles with a working category filter, "Available in Store" label, **no prices** |
| Why shop here | — | 4 feature cards on deep green |
| Google reviews | `#reviews` | 4.1 / 5 · 20 reviews · stars rendered to 4.1 — **no invented quotes** |
| Location | `#location` | Full address, copy-address button, styled map, Get Directions |
| Store hours | `#hours` | Mon–Sun 9:00 AM – 8:00 PM, today highlighted, live **Open now / Closed** pill (Asia/Kolkata) |
| Gallery | `#gallery` | 6 frames with a keyboard-accessible lightbox |
| Before you visit | `#faq` | 5 native `<details>` accordions (also published as FAQ structured data) |
| Final CTA | `#contact` | Get Directions · Explore Categories |
| Footer | — | Brand, quick links, store information, opening hours |
| Mobile action bar | — | Fixed **Directions / Store** bar, phones only, never blocks content |

**Verified business data only** — name, category, address, 4.1★ / 20 reviews, daily
9:00 AM – 8:00 PM. Everything else on the page is either generic website copy or clearly
labelled placeholder content.

---

## Editing content

Everything a client or developer is likely to change lives in one object, `WM_DATA`, near the
bottom of `index.html`:

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

### Add real store photographs

```js
var images = {
  hero:  'images/store-front.jpg',   // replaces the hero artwork
  about: 'images/inside.jpg',        // replaces the about artwork
  store: 'images/counter.jpg',       // fills the location panel
  categories: { 'grocery-food': 'images/grocery.jpg' },
  products:   { 'Rice': 'images/rice.jpg' },
  gallery:    { front: 'images/front.jpg' }
};
```

Set a value and the generated SVG artwork for that slot is replaced by an `<img>`
automatically (with `alt`, `loading="lazy"` and `decoding="async"` already handled).

### Turn the styled map into a live Google map

In Google Maps → **Share → Embed a map → Copy HTML**, then paste only the `src` URL:

```js
var maps = { embedUrl: 'https://www.google.com/maps/embed?pb=…' };
```

The address-based *Get Directions* link already works today; no coordinates or API key needed.

### Show a "View Google Reviews" button

The button is hidden until a real listing URL exists — no fake link:

```js
business.googleBusinessUrl = 'https://www.google.com/maps/place/…';
```

### Re-brand the whole site

Colours, radii, shadows, spacing and type scale are CSS custom properties in one `:root`
block at the top of the file (`--wm-green-*`, `--wm-amber-*`, `--wm-ink-*`). Change ~10 values
and every section, card, button and illustration follows.

---

## Technical notes

- **Responsive:** mobile-first, tested against 320 / 360 / 390 / 430 / 768 / 1024 / 1280 /
  1440 / 1920 widths — dedicated layouts for phones, not a shrunk desktop.
- **Accessibility:** skip link, landmarks, ordered headings, labelled controls, focus-visible
  rings, focus trap + `Esc` on the drawer and lightbox, `aria-live` filter status,
  44px tap targets, and a complete `prefers-reduced-motion` bypass.
- **SEO:** title, meta description, Open Graph, Twitter card, `LocalBusiness` (Store) and
  `FAQPage` JSON-LD containing only verified fields.
- **Performance:** no image requests at all (artwork is inline SVG), no JS bundles, one
  IntersectionObserver for scroll reveals, `requestAnimationFrame`-throttled scroll handler.
- **Graceful degradation:** content renders without JavaScript; the drawer, filters, lightbox
  and live status are progressive enhancements (`<noscript>` fallbacks list categories and
  products as plain links).

## Checklist before going live

- [ ] Photographs: storefront, aisle, counter, and the products you actually stock
- [ ] Google Business Profile URL (enables the "View Google Reviews" button)
- [ ] Optional Google Maps embed URL (replaces the styled map)
- [ ] Confirm the hours and the review count are still current
- [ ] Only if the owner approves: phone / WhatsApp / delivery information
- [ ] Point a domain at the host and add `og:image` once a hosted image URL exists
