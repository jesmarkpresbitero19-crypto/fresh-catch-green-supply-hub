# Fresh Catch and Green Supply Hub

Shopee-style online marketplace for the feasibility study:
**Establishment of Fish and Vegetable Supply and Delivery Services in Binangonan, Rizal.**

No frameworks, no build step, no database — plain HTML + CSS + JavaScript.

## Paano patakbuhin

**Double-click `index.html`.** Yun lang. Isang file lang ang buong website — walang CSS o JS na hiwalay, walang CDN, walang internet na kailangan. Pwede rin i-drag sa browser, i-copy sa USB, o i-attach sa email at gagana pa rin.

Kung gusto ninyo sa localhost (halimbawa para sa demo o defense):

```bash
node server.js
```

Then open <http://localhost:4173>.

## Files

| File | Laman |
|------|-------|
| `index.html` | **Buong website** — HTML, `<style>` (lahat ng CSS), at `<script>` (catalog data + storefront logic) |
| `server.js` | Optional static file server (Node built-ins only) para sa localhost |
| `README.md` | Itong file |

Ang loob ng `index.html`, sunod-sunod:

1. Page markup — top bar, header + search, hero, categories, flash deals, catalog, coverage, footer
2. `<style>` — lahat ng styling at responsive breakpoints (desktop / tablet / mobile)
3. `<script>` — `FCGS_PRODUCTS`, `FCGS_CATEGORIES`, `FCGS_BARANGAYS`, `FCGS_SLOTS`, `FCGS_PAYMENTS`, `FCGS_VOUCHERS`; tapos `ART_KIND` + `ART` (mga larawan ng produkto); tapos ang storefront logic (search, filters, cart, checkout, orders)

## Larawan ng produkto

Bawat produkto ay may sariling **SVG na drawing** — bangus, tilapia, hipon, alimasag, tahong, pusit, pechay, kangkong, karot, kamatis, talong, kalabasa, sili, basket para sa bundles, at iba pa. Hindi emoji at hindi rin naka-link sa internet, kaya gumagana kahit offline at hindi nasisira ang layout.

- `ART_KIND` — mga hugis (fish, shrimp, crab, leafy, carrot, gourd, basket, jar, atbp.)
- `ART` — kulay at hugis ng bawat produkto, naka-key sa product id

Halimbawa, para gawing mas madilim ang tilapia:

```js
'fsh-02': { k:'fish', b:'#b9c2ac', d:'#7c8a6d', s:'round' },
//                     ↑ body       ↑ fins/tail  ↑ slim | oval | round | small
```

Kung gusto ninyong palitan ng totoong litrato: ilagay ang larawan sa tabi ng `index.html`, tapos sa `artSVG()` ibalik ang `'<img src="litrato/bangus.jpg" class="art">'` para sa produktong iyon. Tandaan: kapag naka-hiwalay na file ang larawan, kailangan nang buksan ang site gamit ang `node server.js` (o kasamang i-copy ang folder ng larawan).

## Phone at PC

Iisang file lang, umaayon sa laki ng screen:

| Screen | Ayos |
|--------|------|
| 1400px pataas | 6 na produkto kada hilera |
| Desktop (901–1399px) | 5 kada hilera, nasa gilid ang filter sidebar |
| Tablet (≤900px) | Naka-tago ang filters, may **⚙️ Filters** na button |
| Phone (≤820px) | May bottom navigation bar (Home / Shop / Cart / Orders), mas malalaking pindutan (40px), naka-scroll pahalang ang hot keywords |
| Phone (≤640px) | 2 produkto kada hilera, naka-stack ang cart rows at checkout |
| Maliit na phone (≤380px) | 3 kategorya kada hilera, mas maliit na logo |

Walang horizontal scroll sa 375px at sa 1280px, at may safe-area padding para sa mga iPhone na may notch.

## Features

**Shopping**
- 64 products across 8 categories: Fresh Fish, Seafood & Shells, Leafy Greens, Root Crops, Fruit Vegetables, Herbs & Spices, Meal Bundles, Dried & Preserved
- Live search (name, description, origin, supplier) + hot keywords
- Sidebar filters: category, price range, rating, in-stock only, on-sale only
- Sort: Popular / Latest / Top Sales / Price asc / Price desc, with Load More paging
- Flash deals row with a live countdown timer
- Product detail page: gallery, discount %, stock, per-kilo pricing, quantity stepper, supplier card, specs table, related products

**Cart & checkout**
- Cart with quantity steppers, per-line totals, remove
- Voucher codes: `SARIWA50` (₱50 off ₱400+), `PALENGKE10` (10% off ₱600+, max ₱150), `LIBREPADALA` (free delivery ₱300+)
- Free delivery at ₱500+ with a progress prompt in the cart
- Checkout: name, mobile (09XXXXXXXXX validation), barangay dropdown with per-barangay delivery fee and ETA, street, landmark, delivery date + time slot, notes to the seller
- Payment: Cash on Delivery, GCash, Maya, Bank Transfer
- Order confirmation with order number (`FCG-YYMMDD-NNNN`) and a 4-step tracking bar
- Order history page

**Persistence** — cart and orders are saved in the browser via `localStorage` (`fcgs_cart`, `fcgs_orders`), so they survive a page refresh.

## Business details used in the site

- Delivery coverage: 40 barangays of Binangonan, fees ₱39–₱79 depending on distance
- Delivery slots: 6–9 AM, 9 AM–12 NN, 1–4 PM, 4–7 PM
- Suppliers shown: Pila Pila Fisherfolk Coop, Aling Nena Fish Stall, Rizal Highland Farms, Barangay Growers Group, Highland Produce Traders, Central Luzon Traders
- Address in the footer: Stall 12, Binangonan Public Market, Brgy. Calumpang, Binangonan, Rizal

## Paano baguhin ang produkto

Buksan ang `index.html` sa Notepad o VS Code, hanapin ang `FCGS_PRODUCTS`. One product looks like this:

```js
{ id:'fsh-01', name:'Bangus (Milkfish) — Malaki', cat:'fish', price:180, old:210,
  unit:'kg', sold:1240, rating:4.8, stock:45, emoji:'🐟', bg:1,
  origin:'Laguna Lake, Binangonan', seller:'Aling Nena Fish Stall',
  flash:true, isNew:false, desc:'...' }
```

- `old` — original price; set to `0` if walang diskwento
- `bg` — thumbnail gradient, `1`–`8`
- `flash` — appears in the Flash Deals row
- `stock: 0` — renders as "UBOS NA" and disables Add to Cart

Contact details, prices, and supplier names are placeholders for the feasibility study — palitan ng totoong datos bago gamitin.
