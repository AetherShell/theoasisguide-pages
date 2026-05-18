# The Oasis Guide — Founding Members Charter

This file is the durable record of founding-member agreements. The first entry is the inaugural agreement and locks the founding-member concept into the project.

The data backing each entry lives at `data/pending-founding.json` (pre-tier) or `restaurants.json` (post-tier, with `founding_member: true` preserved). This file is the human-readable record; the JSON is the machine-readable source of truth.

---

## Founding Agreement #1 — 2026-05-14

**Status:** Founding member confirmed. Not yet listed publicly.

**Member:** Stars Shawarma City
**Apple Maps:** https://maps.apple/p/r9bKND0g857SM5

**Origin:** This restaurant is the inspiration for The Oasis Guide. On an early visit, James asked the owner about purified water. The owner subsequently installed a 3M commercial filtration system. That conversation — and the owner's response to it — is the seed event for the entire project. The owner is being recognized as Founding Member #1 because the directory exists in part because of him.

**Current system:** 3M commercial filter (two-stage carbon/sediment). Drinking + bar coverage. No RO. No whole-restaurant point-of-entry treatment.

**Known issues:** Ongoing hard-water complaint from the owner. Refiner conversation is open.

**Tier:** None assigned. Tier requires on-site readings, and the kit visit hasn't happened yet. **Founding badge is independent of tier** — see §Founding Badge Rule below.

**Next action (James):** Return with full test kit — TDS meter, hardness titration, chlorine strips. Take readings at every customer-facing tap (drinking station, bar, ice, kitchen if accessible). Discuss Hyperion residential refiner installation if owner is open to upgrading.

**Out of scope until readings exist:**
- Do not assign a tier
- Do not publish the entry to the live map
- Do not include in any public-facing list

---

## Founding Badge Rule

The `founding_member` boolean is **independent** of the tier system.

- A founding member can wear the founding badge without any tier assigned.
- A founding member's tier, when assigned, follows the normal verification rules (on-site readings, calibrated kit, James personally present).
- A founding member who later fails a tier verification (e.g., readings come back poor) still keeps the founding badge — the badge recognizes the founding act, not the water quality.
- A non-founding restaurant cannot retroactively become a founding member. The founding cohort is closed by event, not by criteria.

**Display rule:** founding members show the founding badge alongside (or above) their tier badge. If no tier exists yet, only the founding badge displays. The founding badge is visually distinct from tier badges — it is not a fourth tier.

---

## Schema impact (for Manus / builders)

Add to the restaurant entry schema:

```typescript
{
  // existing fields: name, address, lat, lng, tier, readings, test_date, photo_url, writeup, restaurant_url
  founding_member: boolean,            // default false
  founding_confirmed_at?: string,      // ISO date if founding_member is true
  founding_confirmed_by?: string,      // attribution
  is_inspiration?: boolean,            // true only for Founding Member #1
}
```

Tier-less founding members live in `data/pending-founding.json` until readings exist. Once a tier is assigned, the entry moves to `restaurants.json` with `founding_member: true` preserved.

---

---

## Schema addendum — 2026-05-17: per-station coverage

The tier ladder (Recommended / Distinguished / Exemplary) is preserved as additive — see the `2026-05-17-brief-tier-system-partial-coverage.md` brief for the rationale on coverage tracking. Note: that brief proposed publishing sub-Recommended listings with coverage maps in lieu of tier badges; James reversed that within the same session. **Recommended is the floor for directory inclusion.** Restaurants below it aren't listed at all. Coverage tracking still applies — the coverage map is the per-station receipt for the tier each listing earned, not a workaround for unfiltered ones.

Add a `coverage` object to each restaurant entry:

```typescript
{
  // existing fields preserved unchanged
  coverage: {
    drinking: "purified" | "tap" | "unknown",   // guest-facing drinking glass
    beverages: "purified" | "tap" | "unknown",  // bar, coffee, juice, soda fountain
    ice: "purified" | "tap" | "unknown",        // any ice served
    cooking: "purified" | "tap" | "unknown",    // kitchen prep, stocks, sauces
    whole_restaurant_poe: boolean               // single POE filter for everything
  }
}
```

Default is `unknown` for any station not yet assessed. Render lean: omit unknown stations from the coverage map; "unknown" in a published receipt reads as a credibility hole.

**Tier derivation rule (coverage is the source of truth; tier is derivable):**

- `whole_restaurant_poe: true` → `Exemplary` (3 drops)
- Else, drinking + beverages + ice + cooking all `purified` → `Distinguished` (2 drops)
- Else, drinking + beverages both `purified` → `Recommended` (1 drop)
- Else → `null` — **not listed in the directory.**

**Display rule:**

- Directory listings (Recommended / Distinguished / Exemplary): render the tier badge plus the per-station coverage map as the receipt.
- Restaurants that don't reach Recommended: no directory listing. Coverage data may still be tracked privately (`pending-founding.json` or equivalent) for narrative or follow-up purposes, but it doesn't go on the public map.

**Founding cohort, separately:** the founding badge is independent of tier per the existing Founding Badge Rule. Founders who also earn a directory listing appear in both the founders section and on the directory map. Founders who haven't earned a directory listing appear in the founders section with the founding badge and the origin note — no coverage map, no tier label. The founding badge recognizes the founding act, not the water quality.

---

*Last updated: 2026-05-17. Maintained as a charter — entries are append-only and dated. Founding moments are historical events, not editable records.*
