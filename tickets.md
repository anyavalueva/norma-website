# Tickets: Norma SEO & organic-booking growth

Tracer-bullet slices derived from [`docs/seo-spec.md`](docs/seo-spec.md). Each ticket cuts a complete,
verifiable path; work the **frontier** — any ticket whose blockers are all done — one at a time with
`/implement`, clearing context between tickets.

**Standing rule (truth constraint):** publish content only for amenities that physically exist.
Dog-friendly is true today. **Barrel sauna and EV charger are planned, not installed — no ticket may
reference them until they are real.**

Two tickets need the owner's credentials and are marked _(owner action)_: the Vercel connect and the
Search Console DNS record. Two tickets are _(owner-led)_ — real work, but not agent-implementable.

---

## Cut over to staywithnorma.com as the canonical home

**What to build:** The finished site is served from the apex domain via Vercel auto-deploying this repo;
the stale "opening soon" waitlist is gone; exactly one host serves the content.

**Blocked by:** None — can start immediately. _(owner action: connect the repo to the Vercel project.)_

- [ ] Pushing the default branch auto-deploys to `staywithnorma.com` via Vercel
- [ ] `https://staywithnorma.com` serves the current site (not the "opening soon" waitlist) over HTTPS
- [ ] `www.staywithnorma.com` 308-redirects to the apex
- [ ] The GitHub Pages deployment is disabled and no longer serves a competing copy
- [ ] Emails captured by the old waitlist form are exported and saved before teardown

## Crawl scaffolding — robots, sitemap, self-hosted fonts

**What to build:** Search engines can discover and crawl the site, and fonts load from our own origin.

**Blocked by:** Cut over to staywithnorma.com as the canonical home (the sitemap uses canonical-host URLs).

- [ ] `robots.txt` served at the apex, allowing crawl and pointing to the sitemap
- [ ] `sitemap.xml` lists every canonical-host page and validates
- [ ] Google Fonts replaced with self-hosted font files; no third-party font requests remain

## Measurement — Search Console and CTA conversion event

**What to build:** We can see which queries drive traffic, and count click-throughs to the Airbnb listing.

**Blocked by:** Cut over to staywithnorma.com as the canonical home; Crawl scaffolding — robots, sitemap,
self-hosted fonts. _(owner action: add the Search Console DNS TXT record.)_

- [ ] Search Console domain property verified; sitemap submitted; indexing requested for the homepage
- [ ] Vercel Analytics enabled
- [ ] Each "Book a stay" link fires a tracked outbound-click event visible in analytics

## Homepage search legibility

**What to build:** The homepage tells search engines and social unfurlers what it is and where, with a
correct heading structure — no visual redesign.

**Blocked by:** None — can start immediately.

- [ ] Exactly one `<h1>` stating what/where; heading hierarchy correct beneath it
- [ ] `<title>` and meta description rewritten (no "opening summer 2026"); "Norma" always qualified with
      cabin + location
- [ ] Canonical, Open Graph, and Twitter card tags present, with a dedicated 1200×630 share image
- [ ] Favicon present
- [ ] Footer Instagram link points to `instagram.com/staywithnorma`
- [ ] Page renders visually unchanged

## VacationRental structured data

**What to build:** The homepage carries valid JSON-LD describing the property so Google understands the
entity.

**Blocked by:** Homepage search legibility (same-file chain).

- [ ] `VacationRental` (or nearest valid lodging type) JSON-LD present, with location and only true
      amenities (dog-friendly; **not** sauna/EV)
- [ ] Passes Google's Rich Results Test and the schema.org validator with no errors

## Photos as responsive images + weight cut

**What to build:** Photos become real, described images that load fast on mobile.

**Blocked by:** VacationRental structured data (same-file chain).

- [ ] Background-image photos converted to `<img>` with descriptive alt text, rendering identically
- [ ] Hero and carousel images recompressed to modern formats with responsive `srcset`
- [ ] The deer video has a poster frame and stays deferred
- [ ] Lighthouse mobile Core Web Vitals pass; total transfer materially lower (~11 MB → ~2.5 MB target)

## Prefactor — shared stylesheet and reusable page skeleton

**What to build:** The homepage's design system is extracted so new pages reuse it instead of duplicating
it. No visible change. _"Make the change easy, then make the easy change."_

**Blocked by:** Homepage search legibility; VacationRental structured data; Photos as responsive images +
weight cut.

- [ ] Inline CSS moved to a linked stylesheet; homepage renders pixel-identical
- [ ] A documented page skeleton exists (head / meta / canonical / schema / responsive-image patterns) that
      a new page copies to inherit both the design system and the SEO baseline

## Guide section — hub and core destination pages

**What to build:** A guide hub plus the first destination pages, each ranking for a single intent and
funneling to the listing. The destination set is **expandable** — more pages get filed later from a
running backlog.

**Blocked by:** Prefactor — shared stylesheet and reusable page skeleton.

- [ ] `/guide/` hub page in the design system, linked from nav and footer, listing the guides
- [ ] Destination pages for Salt Creek / Striped Peak, Lake Crescent / Devil's Punchbowl, and La Push —
      each one search intent, first-person content, a Book-a-stay funnel, and valid schema
- [ ] All new pages added to the sitemap and linked from the hub
- [ ] Each page passes head inspection and the Rich Results Test

_Largest ticket; if it exceeds one context window during `/implement`, split it per page — the hub sets
the pattern and each destination follows the same shape._

## Dog-friendly guide + FAQ

**What to build:** The high-value dog-friendly guide and an FAQ page, both linked from the hub.

**Blocked by:** Guide section — hub and core destination pages (the hub must exist to add pages to it).

- [ ] Dog-friendly Olympic Peninsula guide — where dogs *can* go, given the national park's trail bans —
      funneling to the listing
- [ ] FAQ page with valid `FAQPage` JSON-LD
- [ ] Both added to the hub and the sitemap; both pass the Rich Results Test

## Airbnb listing optimization

**What to build:** The Airbnb listing itself is tuned — the parallel track that books most year-one nights.

**Blocked by:** None — can start immediately. _(owner-led; not agent-implementable.)_

- [ ] Keyword-bearing listing title; every amenity checked; instant book enabled
- [ ] Launch pricing set below market until a review base builds
- [ ] Guide content reused in Airbnb's guidebook feature

## Authority outreach

**What to build:** Norma earns placement *inside* the pages that already rank, plus local links and an
Instagram cadence.

**Blocked by:** Homepage search legibility (the site should be presentable before pitching). _(owner-led.)_

- [ ] Pitches sent to authors of "best cabins near Olympic National Park" roundups
- [ ] Pitches sent to design/outdoors press and PNW newsletters
- [ ] Listed in local/tourism directories (visitor bureau, chamber)
- [ ] Instagram posting cadence with geotags; link-in-bio to the site
