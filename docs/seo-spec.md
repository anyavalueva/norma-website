# Spec — Norma SEO & organic-booking growth

_Status: ready-for-agent · Owner: Howard · Property: Norma, a cabin near Port Angeles, WA_

This spec turns the plan agreed in the strategy session into written scope. It is deliberately
outcome-oriented: it names the searches Norma should win, the on-page and infrastructure work that
makes winning possible, and how we will verify each piece. It does **not** pin specific file paths or
code — the current site is a single `index.html`, but that will change as guide pages are added.

## Problem Statement

Norma is a one-of-a-kind cabin, but nobody searching for a place to stay near Olympic National Park
can find it. The finished site is published on a GitHub Pages URL that Google has not indexed, while
the brandable domain the owner already owns — `staywithnorma.com` — still serves a stale "opening
soon" waitlist page from an older deployment. As a result:

- A person Googling **"stay with norma"** or **"norma cabin port angeles"** does not reliably land on
  the real site, and may land on the outdated waitlist instead.
- The people most likely to book — someone planning an Olympic Peninsula trip and searching for things
  to do, dog-friendly stays, or a quiet cabin — never encounter Norma at all.
- Every future backlink, share, or ranking signal risks accruing to a URL the owner does not control,
  meaning the work would have to be redone later.

The owner's real goal is bookings. In year one those bookings flow through the Airbnb listing, so the
site's job is to **capture the demand that OTAs and listicles don't already own, and funnel it to the
Airbnb listing** — while being built so a later pivot to direct booking is cheap.

## Solution

Consolidate onto one owner-controlled canonical home (`staywithnorma.com`, served by Vercel, auto-
deployed from this repo), make that site fully legible to search engines, and grow it from a single
page into a small, expandable set of destination/guide pages that rank for the long-tail searches
Norma can realistically win. In parallel, earn placement _inside_ the pages that already rank (the
"best cabins near Olympic National Park" roundups and design/outdoors press) rather than trying to
outrank Airbnb and Vrbo for head terms that are structurally OTA-locked. Every page funnels to the
Airbnb listing today; the same owned domain and email capture make direct booking via the Hospitable
API a later switch, not a rebuild.

Success, concretely:

- Searching Norma's brand returns the real site at **#1** within a few weeks of launch.
- Norma appears on **page one** for a growing set of destination/guide long-tails over 2–6 months.
- Norma is **featured in** the third-party roundups and articles that own the competitive head terms.
- The Airbnb listing accrues the early reviews that compound into both Airbnb-internal and Google rank.

## Domain vocabulary

Terms used consistently throughout this spec and the tickets derived from it:

- **Canonical home** — `https://staywithnorma.com`, apex, HTTPS, served by Vercel.
- **The listing** — the Airbnb listing (`airbnb.com/rooms/1602635499147739732`); today's booking
  destination and conversion endpoint.
- **Brand SERP** — the search results page for queries containing "norma" + a cabin/location qualifier.
  Note "norma" alone belongs to other entities (an ammunition brand, an Italian hotel); Norma the cabin
  is always referenced as **Norma + cabin + location**.
- **Guide page** — a destination/things-to-do page in the site's existing design language, one search
  intent per page, each funneling to the listing.
- **Head term** — high-volume commercial queries (e.g. "cabin rental port angeles") whose first page is
  owned by OTAs and listicles; **OTA-locked**, never targeted directly.
- **Long-tail** — specific, lower-volume, higher-intent queries (destinations, dog-friendly, specific
  hikes) that a small site can win outright.
- **Truth constraint** — we only publish content for amenities that physically exist. Dog-friendly is
  true today; **barrel sauna and EV charger are planned, not installed, and must not appear in content
  until real.**

## User Stories

### Searcher / prospective guest

1. As someone who heard about Norma by name, I want to search "stay with norma" and land on the real
   site as the first result, so that I can see photos and book instead of hitting a dead waitlist page.
2. As a traveler planning an Olympic Peninsula trip, I want to find a page about a quiet cabin near the
   park, so that I have an alternative to the identical-looking OTA listings.
3. As a dog owner, I want to find a dog-friendly cabin near Olympic National Park and learn where I can
   actually take my dog (the park bans dogs on most trails), so that I can plan a trip that works.
4. As someone deciding between stays, I want a share link that unfurls with a real photo and description
   in messages and social, so that I can send it to the people I'm traveling with.
5. As a mobile user on a slow connection, I want the site to load quickly, so that I don't bounce before
   the hero image appears.
6. As a traveler researching specific spots (Lake Crescent, La Push, Striped Peak, Salt Creek), I want a
   trustworthy first-person guide, so that I plan my days — and notice the cabin those guides belong to.
7. As a screen-reader user, I want images described and headings structured, so that I can navigate the
   page.
8. As a searcher who found a guide page, I want an obvious path to check availability and book, so that
   research turns into a reservation.

### Owner / operator

9. As the owner, I want one canonical URL I control, so that every backlink and ranking compounds on an
   asset I keep — not on a GitHub Pages address.
10. As the owner, I want the site to auto-deploy when I push, so that publishing changes stays as simple
    as it is today.
11. As the owner, I want the stale "opening soon" page taken down, so that it stops competing with and
    undercutting the real site.
12. As the owner, I want to see, in Search Console, which queries bring impressions and clicks, so that I
    can steer content toward what's working.
13. As the owner, I want to measure clicks on "Book a stay," so that I know the site is actually sending
    people to the listing.
14. As the owner, I want to add new destination/guide pages over time from a growing list of places, so
    that the content base expands without re-architecting the site.
15. As the owner, I want to be pitched into the "best cabins near Olympic National Park" articles, so
    that Norma appears on page one for head terms without having to outrank Airbnb.
16. As the owner, I want the site built so that switching to Hospitable-powered direct booking later is a
    CTA/endpoint change, not a rebuild, so that the OTA fee becomes optional down the line.
17. As the owner, I want content limited to amenities that truly exist, so that guests are never misled
    and reviews stay strong.

## Implementation Decisions

### Infrastructure & canonicalization

- **Canonical home is `staywithnorma.com`, served by Vercel**, auto-deploying from this repo. GitHub
  Pages is retired after cutover so only one copy of the site is live. `www` 308-redirects to the apex;
  HTTPS enforced. Rationale: the domain is owner-controlled and Vercel gives redirect/header control
  plus a clean path to later serverless needs (Hospitable).
- **The stale waitlist deployment is replaced**, and any captured waitlist emails are exported before it
  is torn down (small list, preserved for completeness, not a launch lever).
- Two steps require the owner's credentials and are called out as such in the tickets: connecting the
  repo to the Vercel project, and adding the Search Console DNS verification record.

### On-page legibility (homepage)

- The homepage's visual design is preserved. Changes are additive to what crawlers and share-cards read:
  a single true `<h1>`, a corrected heading hierarchy beneath it, a rewritten `<title>` and meta
  description that state what/where and drop "opening summer 2026," and canonical + Open Graph + Twitter
  card tags with a dedicated share image.
- **Photos become real `<img>` elements with descriptive alt text.** They are currently CSS background
  images, invisible to image search and screen readers; they will render identically via object-fit.
- **Structured data:** `VacationRental` (or the closest valid lodging type) JSON-LD describing the
  property, location, and amenities; `FAQPage` JSON-LD ships with the FAQ page. This establishes entity
  clarity now; Google's Vacation Rentals travel module itself is a later, Hospitable-fed integration and
  is out of scope here.
- Standard crawl scaffolding: `robots.txt`, `sitemap.xml`, favicon, self-hosted fonts, per-page canonical.

### Performance

- Total page weight is cut substantially (hero and carousel images recompressed to modern formats with
  responsive `srcset`; the deer video gets a poster frame and stays deferred). Target: pass Core Web
  Vitals on mobile. Rationale: mobile performance is a ranking input and the current ~11 MB payload is
  the biggest single on-page liability.

### Content architecture (expandable)

- The site grows from one page into a **guide section**: a hub plus one page per destination, each
  targeting a single long-tail intent and funneling to the listing. The initial set is drawn from the
  places already named on the homepage (Salt Creek / Striped Peak, Lake Crescent / Devil's Punchbowl, La
  Push, plus a dog-friendly-peninsula guide and an FAQ).
- **The destination list is open-ended by design.** Additional guide pages (more hikes, beaches, towns,
  seasonal itineraries) are added over time from a running backlog; the information architecture and URL
  scheme must accommodate growth without restructuring. New pages reuse the homepage's design system and
  the same funnel pattern.
- Homepage copy gains natural, truthful mentions of "dog-friendly" and specific location phrasing it
  currently omits.
- **Brand-reference rule** is applied everywhere: "Norma" is always qualified with "cabin" and a location
  so the brand SERP disambiguates from unrelated "Norma" entities.

### Authority & distribution (owner-led, ongoing)

- Outreach to the authors of existing "best cabins near Olympic National Park" roundups to get Norma
  included — borrowed page-one presence for OTA-locked head terms.
- Pitches to design/outdoors press and PNW newsletters whose audience matches Norma's aesthetic.
- Local/tourism directory links (visitor bureau, chamber).
- Instagram (`@staywithnorma`) treated as a real channel: fixed footer link, geotagged posts, link-in-bio
  feeding brand searches.

### Airbnb-internal (parallel track)

- Treated as first-class, because in year one it books more nights than Google: keyword-bearing listing
  title, complete amenity data, instant book, fast response times, launch pricing under market until a
  review base builds, and reuse of the guide content in Airbnb's guidebook feature.

### Forward-compatibility

- Email capture and the owned domain are retained so that direct booking via the **Hospitable API**
  later is a CTA-and-endpoint change. No direct-booking, payment, or tax machinery is built now.

## Testing Decisions

Good tests here assert **externally observable search/rendering behavior**, not implementation details.
The site is static, so verification leans on standard SEO validators and real-world crawl feedback
rather than a unit-test suite. Seams, highest first:

- **Primary seam — the rendered page `<head>` + JSON-LD.** For any built page, assert the presence and
  correctness of: exactly one `<h1>`, `<title>`, meta description, canonical, Open Graph/Twitter tags,
  and a JSON-LD block that passes **Google's Rich Results Test** and the schema.org validator with no
  errors. This single seam covers most "is Google reading this right" risk and should be checked on every
  page before it ships.
- **Performance seam — Lighthouse / PageSpeed Insights (mobile).** Core Web Vitals pass and total transfer
  size drops meaningfully versus the current page. Run against the deployed canonical URL.
- **Indexing seam — Search Console.** URL Inspection shows pages indexed on the canonical host; the
  sitemap is read without errors; Coverage stays clean. This is the lagging, real-world confirmation.
- **Link & funnel integrity.** Internal guide links resolve, there are no dead links, and every "Book a
  stay" CTA resolves to the live listing. A click event on the CTA is measurable in analytics.
- **Redirect/canonical correctness.** `www` → apex and any old GitHub Pages path behave as intended; only
  one host serves 200s for the content.

Prior art: none in-repo yet (no test suite exists). These are external-tool checks; where a repeatable
scripted check is cheap (e.g. asserting head tags exist in the built HTML), prefer it, at the highest
seam — the rendered page — rather than testing template internals.

## Out of Scope

- Direct booking, payments, damage deposits, and lodging-tax mechanics (deferred to the later Hospitable
  phase).
- Google's Vacation Rentals travel module / Hotel Center feed (requires the direct-booking integration).
- Google Business Profile (single vacation rentals are ineligible).
- Content for **barrel sauna and EV charger** until those amenities physically exist.
- Paid search / Google Ads and any paid SEO tooling subscription (measurement stays on the free stack;
  a single paid month may be revisited mid-content-phase only if we're flying blind).
- Outranking OTAs/listicles for head terms — the strategy is to be featured within them, not to beat them.
- A CMS or framework migration; the site stays static and file-based.

## Further Notes

- **Sequencing.** Cutover (replace the stale waitlist, canonicalize on Vercel, retire Pages) is the most
  urgent item and is independent of the rest — the stale page costs bookings today regardless of SEO.
  On-page legibility and performance land next as one pass. Guide pages and authority outreach then run
  in parallel and continue indefinitely.
- **Expectations.** Indexing within days of the legibility pass; brand SERP #1 in ~2–4 weeks; long-tail
  wins compounding over 2–6 months; roundup/press placements as they're earned. Head terms remain
  OTA-locked for everyone — Norma's presence there is _inside_ Airbnb's and the listicles' page-one
  results, which is where those searchers click anyway.
- **Measurement stack (free):** Search Console (queries/impressions/clicks, weekly), Vercel Analytics with
  a CTA click event as the conversion proxy, and a monthly manual rank check on a tracked query set.
- **Truth constraint is a standing rule,** not a one-time note: any content ticket that references sauna or
  EV must be blocked until the amenity is installed.
