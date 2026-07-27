# Where the catalogue numbers come from

Provenance for every figure hard-coded in `index.html`. Verified **27 July 2026**.

Published in machine-readable form as
[danielrosehill/storage-container-dimensions](https://huggingface.co/datasets/danielrosehill/storage-container-dimensions)
on Hugging Face — same figures plus the VDA 4500 KLT sizes and per-row
standards-conformance fields. `build.py` in that repo is the generator; if a
capacity changes, change it there and mirror it into the `GROUPS` array here.

The calculator's whole point is comparing containers on *usable* capacity, so the
capacity figures matter more than the dimensions. Getting them wrong by 10 % changes
which box wins a comparison.

## The two families

| | Euro stacking container | Attached-lid container (ALC) |
|---|---|---|
| Walls | straight | sloping (tapered towards the base) |
| Lid | separate purchase | integral, hinged, included in the price |
| Stacks when full | yes | yes (typically 9 high) |
| Nests when empty | **no** | **yes, ~75 %** |
| Usable / external | ~76–78 % | ~73–75 % |

That table is the answer to "are ALCs Euroboxes?" — no. They share the 600 × 400
and 400 × 300 footprints and interoperate on the same pallets, but they are a
different product with a different height grid and different volumetrics. Hence
the tool being renamed away from "Eurobox calculator".

## Euro stacking containers

Capacities are computed from published internal dimensions rather than copied from
vendor "nominal litres", which round up by 5–10 %.

The internal footprint is remarkably consistent across manufacturers:

- **600 × 400** boxes are internally **550 × 355 mm**, internal height ≈ `H − 15 mm`
- **400 × 300** boxes are internally **350 × 250 mm**, internal height ≈ `H − 10 mm`
- **300 × 200** boxes are internally **260 × 160 mm**, internal height ≈ `H − 10 mm`
- **800 × 600** boxes are internally **750 × 550 mm**, internal height ≈ `H − 20 mm`

Cross-checks against real SKUs (all agree within ~1 L):

| SKU | External | Published internal | Published L | Formula gives |
|---|---|---|---|---|
| Yossibox G-4623 | 600 × 400 × 230 | 550 × 360 × 215 | 42 | 42.0 |
| Yossibox G-4634 | 600 × 400 × 340 | 550 × 360 × 325 | 64 | 63.5 |
| Yossibox T-4631 | 600 × 400 × 315 | — | 58 | 58.0 (at H 310) |
| plasticboxshop 60 L | 600 × 400 × 320 | 550 × 350 × 300 | 60 | 59.6 |
| plasticboxshop 76 L | 600 × 400 × 400 | 550 × 350 × 390 | 76 | 79.1 |
| plasticboxshop 20 L | 400 × 300 × 220 | 350 × 250 × 210 | 20 | 18.4 |

The 800 × 600 figures are the weakest of the set — they are formula-derived and not
cross-checked against a published internal dimension. Thin-walled Rako 800 × 600
containers are marketed at ~134 L against the formula's 124 L, so treat the
800 × 600 row as conservative.

Sources: [`../../Eurobox-Procurement-Research/research/06-yossibox-rigid-skus.md`](../../Eurobox-Procurement-Research/research/06-yossibox-rigid-skus.md)
(Israeli retail, verified 2026-07-23) and the plasticboxshop.co.uk Shopify catalogue
(see "Pulling supplier catalogues" below).

## Attached-lid containers

ALC capacities are the manufacturers' published nominal figures. Unlike the Euro
boxes these cannot be derived geometrically — the wall taper is not published, and
it varies down the range.

**600 × 400** — the mainstream sloping range, mutually stackable and nestable
across Loadhog and Schoeller Allibert Kaiman:

| External H | Nominal L | Usable / external |
|---|---|---|
| 250 mm | 44 | 73 % |
| 310 mm | 56 | 75 % |
| 367 mm | 65 | 74 % |
| 400 mm | 80 | 83 % |

**400 × 300**: 222 mm → 22 L, 264 mm → 25 L, 306 mm → 30 L.

Two cautions, both confirmed rather than inferred:

- **The 400 mm / 80 L entry is an outlier** at 83 % efficiency where the rest of the
  range sits at 73–75 %. Either it has less taper or the 80 L is a generous round
  number. It is in the catalogue as published; do not read a 60 × 40 × 40 ALC as
  genuinely more space-efficient than its siblings without measuring one.
- **Nominal capacity for the same external size varies by up to 6 % between
  ranges.** At 600 × 400 × 310 mm: Loadhog 56 L, "CS" heavy-duty 55 L, recycled
  crocodile-lid 53 L. The catalogue uses the Loadhog/Kaiman figure. For a real
  purchase decision, use the specific supplier's number.

The **75 % nesting ratio** is from vendor copy ("nest up to 75 % when empty") and is
what drives the "Empty, nested" figures. Euro stacking containers are modelled as
not nesting at all, which is right for straight-walled solid boxes but wrong for the
bi-colour stack/nest variants — those are not in the catalogue.

The Kaiman height grid (264 / 308 / 365 / 400 at 600 × 400, and 222 / 264 / 306 at
400 × 300) came from IPL Schoeller's sitemap and product pages. Their product pages
publish **external dimensions only**; internal dimensions and nesting ratios are in a
leaflet PDF behind a lead-capture form, which is why the capacities here come from
retailers instead.

## Pulling supplier catalogues

The useful trick, and the one worth remembering: **plasticboxshop.co.uk runs on
Shopify**, so its full catalogue including prices is available as JSON with no key
and no scraping:

```bash
curl -s "https://www.plasticboxshop.co.uk/collections.json?limit=250"
curl -s "https://www.plasticboxshop.co.uk/collections/alc-containers/products.json?limit=250"
curl -s "https://www.plasticboxshop.co.uk/collections/euro-stacking-containers/products.json?limit=250"
```

`body_html` carries the spec prose (internal and external dimensions, capacity,
nesting and stacking claims); `variants[].price` carries the price. Any Shopify
storefront answers `/products.json` and `/collections.json` the same way — check for
`sitemap.xml` naming `sitemap_products_*.xml` to identify one.

What did **not** work, so as not to repeat it:

- DuckDuckGo HTML, Bing and Mojeek all return bot challenges to `curl`.
- `schoellerallibert.com` product paths 404; the live site is `iplschoeller.com`,
  reachable via `schoellerallibert.com/sitemap.xml` → `iplschoeller.com/sitemaps/en.xml`.
- BigDug and similar render their category pages client-side — the HTML has no
  product data in it.
- `hudsonexchange.com` has no `sitemap.xml` and 404s on guessed collection paths.

## Volume price

The comparison metric — purchase price ÷ usable capacity, in currency per usable
litre, with the same number per m³ alongside.

There is no industry-standard name for this. Freight and self-storage quote per m³,
supermarket unit pricing does the same job for groceries, and container
manufacturers quote per box. The term "volume price" is this tool's own.

Litres are the primary scale because every container on the market lands between
roughly 0.10 and 10 per litre in any major currency, which reads better than the
four-digit figures the per-m³ form produces.

Deliberately **not** modelled: freight and duty (which scale with external volume,
not usable, and would favour the Euro box further), lid cost for Euro boxes bought
with lids (add it to the unit price yourself — an Israeli 60 × 40 lid is about ₪23),
and any bulk-break pricing. Currencies do not convert; one comparison is one
currency, because a hard-coded FX rate would silently rot.
