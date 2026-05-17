# The Oasis Guide — Vision Handoff for Manus

**Prepared:** 2026-05-14 by Sofia / Claude Code, for James Arkell
**Repo:** `/home/sophia/projects/oasis-guide/` (git tracked)
**Live target:** theoasisguide.com (GitHub Pages, AetherShell org)
**Parent brand:** Safe Water Southwest (credited in footer)

---

## 1. What you're building

An editorial guide — Michelin-flavored, not utility — to Phoenix-metro restaurants serving clean water. Each listing is **verified in person** by James with calibrated TDS + pH meters at every customer-facing tap (bar, ice, drinking station, kitchen). Readings are published openly on the listing. That on-site verification is the entire differentiator vs. Yelp/Seed-Oil-Scout-style community attestation.

**Tone:** aspirational, curatorial. Never fear-based. Diners pick from desire, not anxiety. ("Safe water" framing repels the restaurant audience — that copy stays on the residential SWSW lead-gen site.)

**Audience priority:** diners discover the site; restaurants are the ones who need to be sold. Restaurant signup converts the whole funnel.

---

## 2. Brand lock (do not redesign without James)

Locked 2026-05-13. Brief charcoal+copper detour was rolled back to blue+gold on 2026-05-14.

### Palette
| Token | Hex | Use |
|---|---|---|
| `--ink` | `#13355c` | Deep cool blue — primary background, headers |
| `--ink-deep` | `#0a2240` | Footer, body text |
| `--gold` | `#d4af37` | Antique gold — drops, accents, trim |
| `--gold-soft` | `#e6c763` | Hover/highlight gold |
| `--cream` | `#fafbfc` | Page background |
| `--cream-line` | `#e2e8f0` | Dividers |
| `--muted` | `#64748b` | Subtitles, secondary text |
| Single-color print | `#1a3a4a` on cream | Menu badges, B&W tier marks |

### Typography
- **Serif everywhere:** Georgia / Times New Roman fallback. Editorial feel.
- Letter-spacing tracked open on headings (`0.04em`–`0.12em`).
- Italic for accents (`em` tags get `--gold-soft` color in headings).
- No sans-serif anywhere in v1.

### Brand mark — locked: Option C, hex geometric
- Hexagonal frame
- Top to bottom inside hex: "THE OASIS GUIDE" wordmark → thin divider → 1/2/3 water drops centered → thin divider → tier name in bold
- Drops are the visual focal point
- Reusable `#drop` SVG symbol + `#oasis-mark` symbol defined in `index.html` (lines 335–351) — canonical reference
- See `badge-mockups.html` Option C section for the locked direction (Options A and B kept on the page as comparison only — do not use)

---

## 3. Tier model (locked names)

Three tiers, named Michelin-style. **Tier badges are earned, not bought.** On-site test is the only way to move up.

| Tier | Drops | Meaning |
|---|---|---|
| **Recommended** | 1 | Purified drinking water + beverages. Tap for cooking and ice. "We take water seriously" floor. |
| **Distinguished** | 2 | Purified drinking, beverages, ice, AND cooking water. Kitchen + bar both clean. |
| **Exemplary** | 3 | Whole-restaurant purification at point of entry. Every tap, every appliance. The unicorn. |

Optional **alkaline** orthogonal tag — descriptive only ("offers alkaline pH 8.5+"). Never endorse health claims. Credibility risk if SWSW backs contested claims.

---

## 4. Architecture & stack decisions

- **Independent micro-site at theoasisguide.com** — NOT a subpath of safewatersouthwest.com. Restaurants need to pitch a brand, not a URL path.
- **GitHub Pages + Namecheap DNS** — repo `AetherShell/theoasisguide-pages` (per DNS notes). DNS records in `shared/dispatch/oasis-guide/dns-namecheap-records.md`.
- **Map:** Leaflet 1.9.4 + OpenStreetMap tiles. No API key, no recurring cost. Centered Phoenix metro `[33.45, -112.0]`, zoom 10, scroll-wheel zoom disabled (annoying on long pages).
- **Restaurant data:** JS array of objects mirroring the v6.html vendor-directory pattern in `projects/water-database/`. Each entry: `name`, `address`, `lat`, `lng`, `tier`, `readings` (array of tap-by-tap TDS/pH), `test_date`, `photo_url`, `writeup`, `restaurant_url`, **`founding_member` (boolean), `founding_confirmed_at`, `founding_confirmed_by`, `is_inspiration`**.
- **Founding members:** A restaurant flagged `founding_member: true` displays a founding badge **independent** of tier status — see `FOUNDING-MEMBERS.md` for the charter rule. Founding members without verified tier readings live in `data/pending-founding.json`; when readings are taken and a tier is assigned, the entry promotes to `restaurants.json` with `founding_member: true` preserved. Founding badge is visually distinct from tier badges (not a fourth tier). A founding member who later fails tier verification keeps the founding badge — it recognizes the founding act, not the water quality.
- **Signup backend:** existing Google Sheets pattern (the same one SWSW residential uses). Apps Script webhook → sheet row.
- **Photos:** restaurant-provided URLs first; if needed, GitHub repo `/photos/`. Skip S3 in v1.
- **Hand-curated launch:** 3–5 of James's relationship restaurants. Brunch (Phoenix) is the first warm lead.

### Files in repo today
```
oasis-guide/
├── CNAME                    ← "theoasisguide.com" for GitHub Pages
├── index.html               ← live homepage (v1, empty-state directory)
├── badge-mockups.html       ← 3 badge directions; Option C is locked
├── pitch-onepager.html      ← printable letter-size pitch sheet
└── VISION-HANDOFF-FOR-MANUS.md   ← this file
```

### Companion docs (in `~/shared/dispatch/oasis-guide/`)
- `dns-namecheap-records.md` — paste-ready DNS record set
- `pitch-crib-sheet.md` — phone-friendly 30s and 2-min pitch scripts
- `outreach-templates.md` — 6 email/SMS/in-person templates with discipline rules
- `target-list-week1.md` — Tier A (5 restaurants), Tier B (10), coffee annex (8), skip list

---

## 5. Pitch discipline — locked 2026-05-13

**v1 pitch framing is founding-restaurant perk only.** Featured placement is framed as a permanent thank-you to founding listings. NEVER as a future paid product.

### Forbidden in v1 outreach
- "Paid placement," "Featured listing fee," "Premium tier"
- "Vendor referrals," installer commissions
- "Industry partnership," "Sponsorship"
- "Best of Phoenix" / awards language
- Any health claims about alkaline / mineral water
- "Safe water" framing toward the restaurant (fear-based; reserved for residential SWSW)

### Why
Tomas (Hyperion exec) called SWSW's name fear-based 2026-05-12. James agreed it works for residential lead-gen but not for restaurants. Sub-brand resolves the tension. Parallel discipline for Oasis Guide: don't lead with money.

### When monetization launches (v2+)
- Goes into FAQ / about copy with clear disclosure
- NOT into the in-person pitch
- Disclosure stays on home/category pages, NEVER on listing pages (Michelin took years of damage for letting paid placement leak into the guide)

---

## 6. Monetization (sequenced, OUT OF v1)

Both paths exist but neither is wired in v1. Building the asset first, proving the pitch loop, then turning on revenue.

1. **Vendor referrals** (first) — when restaurants want to upgrade tiers, SWSW refers to vetted commercial installers from the existing vendor directory. Per-close referral fee. No vendors on terms yet. Recruit AFTER 5+ restaurants are listed; the pitch becomes "I have N restaurants wanting upgrades — want exclusive Phoenix referral rights?"
2. **Featured restaurant placement** (second) — paid, clearly disclosed. Same tier badge — money can't buy a higher tier. Hold until 10–15 restaurants live; nobody pays for placement in an empty directory.

---

## 7. Standing constraints

- **No fear-based copy.** Aspirational only. The guide is about finding good water, not avoiding bad water.
- **No alkaline health claims.** Descriptive tag only.
- **No pay-to-tier.** Featured placement OK with disclosure; tier badge is earned by on-site test only.
- **Disclosure rule:** on home/category pages, NEVER on listing pages.
- **Annual re-test** to keep listing current — written into the pitch.
- **Each location verified separately** — chains list locations, not brands.

---

## 8. Visual reference — badges and brand mark (inline SVG)

The hex brand mark and the three tier badges below are the canonical art. All defined as reusable SVG `<symbol>`s in `index.html`. Copy the markup directly; do not re-trace.

### Core reusable symbols (place once in DOM)

```html
<!-- Water-drop symbol -->
<svg width="0" height="0" style="position:absolute" aria-hidden="true">
  <symbol id="drop" viewBox="-10 -14 20 30">
    <path d="M 0,-12 C 7,-2 8,4 7,9 C 7,13 4,15 0,15 C -4,15 -7,13 -7,9 C -8,4 -7,-2 0,-12 Z"
          fill="currentColor"/>
  </symbol>
</svg>

<!-- Brand mark (header logo, generic) -->
<svg width="0" height="0" style="position:absolute" aria-hidden="true">
  <symbol id="oasis-mark" viewBox="0 0 200 200">
    <polygon points="100,15 175,55 175,145 100,185 25,145 25,55" fill="#13355c"/>
    <polygon points="100,28 162,62 162,138 100,172 38,138 38,62"
             fill="none" stroke="#d4af37" stroke-width="0.8"/>
    <g color="#d4af37">
      <use href="#drop" x="89" y="78" width="22" height="44"/>
    </g>
  </symbol>
</svg>
```

### Tier-specific badge markup

Each tier badge: hex outer + hex inner trim + wordmark + drops + tier name. Substitute 1/2/3 `<use href="#drop">` calls and change the bottom `<text>` to the tier name.

**Recommended (1 drop):**
```html
<svg viewBox="0 0 200 200" width="180" height="180">
  <polygon points="100,15 175,55 175,145 100,185 25,145 25,55" fill="#13355c"/>
  <polygon points="100,28 162,62 162,138 100,172 38,138 38,62"
           fill="none" stroke="#d4af37" stroke-width="0.8"/>
  <text x="100" y="72" text-anchor="middle" fill="#d4af37"
        font-family="Georgia, serif" font-size="9" letter-spacing="2.5">THE OASIS GUIDE</text>
  <line x1="60" y1="80" x2="140" y2="80" stroke="#d4af37" stroke-width="0.4" opacity="0.7"/>
  <g color="#d4af37">
    <use href="#drop" x="89" y="86" width="22" height="30"/>
  </g>
  <line x1="60" y1="122" x2="140" y2="122" stroke="#d4af37" stroke-width="0.4" opacity="0.7"/>
  <text x="100" y="137" text-anchor="middle" fill="#d4af37"
        font-family="Georgia, serif" font-size="10" letter-spacing="2" font-weight="bold">RECOMMENDED</text>
</svg>
```

**Distinguished (2 drops):** replace the single `<use>` block with:
```html
<g color="#d4af37">
  <use href="#drop" x="76" y="86" width="22" height="30"/>
  <use href="#drop" x="102" y="86" width="22" height="30"/>
</g>
```
…and the bottom text with `DISTINGUISHED`.

**Exemplary (3 drops):**
```html
<g color="#d4af37">
  <use href="#drop" x="63" y="86" width="22" height="30"/>
  <use href="#drop" x="89" y="86" width="22" height="30"/>
  <use href="#drop" x="115" y="86" width="22" height="30"/>
</g>
```
…and `EXEMPLARY`.

### Single-color print variant
Same hex, no fill, all strokes/text in `#1a3a4a` on cream `#f6f1e6`. Use for menu badges and printable window decals. See `badge-mockups.html` Option C "Menu mark — single color."

---

## 9. Listing page (NOT yet built — Manus should design this)

Each restaurant listing should be its own page, not a card on a master list. The directory page is the map + index; the listing page is the editorial unit.

Suggested layout per listing:

- **Hero strip:** hero photo of the restaurant (interior or filtration setup), tier badge in the corner.
- **H1:** restaurant name + tier line (e.g., "Christopher's at Wrigley Mansion — Distinguished")
- **Lede paragraph (~80 words):** editorial writeup. What kind of restaurant, what the chef cares about, why the water is part of that story. James's voice.
- **The readings table:** every tap tested, TDS in ppm, pH, test date. Calibrated meter model footnoted.
  - Bar
  - Ice machine
  - Drinking-water station
  - Kitchen prep tap
  - Coffee station (if applicable)
- **Setup description:** what the restaurant runs (filter brand, RO type, point-of-entry vs. point-of-use). Restaurant-supplied, James-verified.
- **Photos:** filtration installation if accessible, restaurant interior, signature dishes.
- **Sidebar/footer:** address, phone, hours, website, "Last verified [date]" + "Next re-test scheduled [date]."

**Editorial discipline:** never compare restaurants. Each listing stands alone. No "better than" or "best of" language.

---

## 10. Open scope for Manus

Things explicitly NOT decided yet — these are Manus's calls (or James's, route ambiguous ones back):

1. **Listing page template** — design and layout per §9. Build a reusable component.
2. **Per-restaurant URL structure** — `/restaurants/[slug]/` recommended for SEO.
3. **CMS/data layer** — Google Sheets (matching SWSW pattern) is the simplest, but if Manus wants a flat JSON file in the repo for v1 with 3–5 hand-curated entries, that's also fine. Avoid recurring-cost services.
4. **Photo workflow** — restaurant uploads to a Google Drive folder → James moves verified ones into the repo? Or photo URLs only? Pick the lowest-friction path.
5. **SEO page generation** — static at build time, vs. client-side rendering. Static preferred for indexability.
6. **Search/filter UI** — by neighborhood, cuisine, tier. Defer if it costs time.
7. **Restaurant onboarding flow after signup** — automated email confirmation? Or James personally calls back within 48 hours (current promise on the form)? The latter is the v1 expectation.

---

## 11. Out of scope for v1 (do not build)

- Commercial water treatment sales process automation
- Payment processing (no paid listings v1)
- Mobile app
- User reviews / UGC
- National expansion beyond Phoenix metro
- POS / menu system integrations
- Email marketing automation to listed restaurants
- Featured-placement billing or vendor-referral tracking

These are v2+. The first version's job is to prove the awareness/SEO/B2B-bridge hypothesis with minimal infrastructure.

---

## 12. Escalation triggers (Manus pause and ping James if…)

- Tooling choice introduces a recurring cost not anticipated
- Restaurant data needs legal review (claims about purity, liability)
- Build time exceeds 8 hours before any restaurant is live
- Anything in §5 (pitch discipline) starts to leak into v1 site copy

---

## 13. Success criteria (60-day pilot)

- 3–5 restaurants live and verifiably accurate
- Locator pages indexed by Google
- First inbound from a Phoenix restaurant outside James's network expressing interest in being listed

Post-pilot (next 90 days):
- 10+ restaurants
- Measurable organic traffic
- First commercial water-treatment conversation initiated from a listed restaurant

---

## 14. Pointers / canonical references

- **Repo:** `/home/sophia/projects/oasis-guide/` (commits visible via `git log`)
- **Live homepage source:** `index.html` (current v1, empty-state directory + signup form)
- **Pitch sheet source:** `pitch-onepager.html` (letter, print-ready)
- **Badge canonical art:** `badge-mockups.html` — Option C section only
- **DNS:** `~/shared/dispatch/oasis-guide/dns-namecheap-records.md`
- **Pitch scripts:** `~/shared/dispatch/oasis-guide/pitch-crib-sheet.md`
- **Outreach templates:** `~/shared/dispatch/oasis-guide/outreach-templates.md`
- **Week-1 target list:** `~/shared/dispatch/oasis-guide/target-list-week1.md`

---

## 15. One-paragraph summary for Manus

You are finishing the build of an editorial restaurant guide (theoasisguide.com) for Phoenix restaurants serving clean water, verified in person by the founder with calibrated meters. The visual identity is locked — deep blue + antique gold, Georgia serif, hex-framed brand mark with 1/2/3 water drops indicating tier (Recommended / Distinguished / Exemplary). The homepage, badge mockups, and pitch sheet are already built and live in the repo. What's missing: the listing-page template (one page per restaurant, editorial writeup + meter-readings table + photos), the data layer to drive listings, and the live wiring of 3–5 founding restaurants. Pitch discipline is strict — no monetization language, no fear-based copy, no health claims. Hold the bar high; this guide is meant to feel curated, not commercial.
