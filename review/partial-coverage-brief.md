> **SUPERSEDED — 2026-05-17 (same day).** James reversed the central proposal in this brief: "we're not going to be listing people who don't meet the bar." Recommended is the floor for directory inclusion. Sub-Recommended restaurants do NOT get directory listings (with or without coverage maps). Founders below the floor get founder-card-only recognition (founding badge + origin note, no map pin, no tier label). The per-station coverage map is preserved as a feature of directory listings only — it's the receipt for the tier the listing earned, not a workaround for restaurants below the bar.
>
> Current canonical rule lives in `FOUNDING-MEMBERS.md` schema addendum (2026-05-17) and in `VISION-HANDOFF-FOR-MANUS.md` §3, §4, §7, §9. Site copy is aligned in `index.html`, `pitch-onepager.html`, `pitch-crib-sheet.md`, `outreach-templates.md`.
>
> The brief below is preserved as the historical record of what was proposed; do not act on its display rule.

---

# Brief — Oasis Guide tier system revision for partial coverage

**To:** the Logos window actively upgrading theoasisguide.com
**From:** Logos on Penguin, 2026-05-17
**Working directory you want:** `sophia:~/projects/oasis-guide/`
**Site lives at:** theoasisguide.com (GitHub Pages, `AetherShell/theoasisguide-pages` — repo path may differ from local dir name, verify before pushing)

---

## Why this work exists

The current three-tier ladder (Recommended / Distinguished / Exemplary) is additive: each tier builds on the previous. Recommended = purified drinking + beverages. Distinguished = + ice + cooking. Exemplary = whole-restaurant point-of-entry.

That ladder assumes the natural direction of restaurant water investment is drinking-water-first, then expanding outward. The economic reality is the opposite: commercial filtration is almost always driven by **equipment protection** (dishwasher warranties, combi-oven scale, ice-machine longevity, espresso quality), not by diner-facing drinking water. So a restaurant that has invested in filtration *at all* is likely to have cooking/equipment lines filtered before the diner's glass.

Founding Member #1, Stars Shawarma City (owner: Nikola), is our first concrete instance: 3M two-stage commercial filter feeding cooking and equipment, drinking water still tap. James kit-visit happens today. SSC fails Recommended (no drinking-water purification) but is meaningfully more invested than an unfiltered restaurant. The current schema and rendering have no honest way to show that — SSC either gets a tier it hasn't earned or appears tier-less with no explanation of what it actually has.

James's editorial call: **keep the additive tier ladder** (Option A from the strategy discussion). Don't redesign tiers as orthogonal facets. Instead, **always render an explicit per-station coverage map** as the verification receipt, regardless of whether the listing crosses a tier threshold. Restaurants below Recommended show the coverage map without a tier badge. Restaurants at or above Recommended show the coverage map alongside their tier badge — the tier is the rollup; the coverage map is the receipt.

This preserves "drops are earned breadth" semantics for the prestige ladder while letting partial-coverage restaurants be honestly published.

---

## Canonical files to read before changing anything

- `FOUNDING-MEMBERS.md` — charter, append-only, narrative authority for founding agreements. Read the existing FM#1 entry and the Founding Badge Rule before touching tier logic.
- `data/pending-founding.json` — machine source of truth for tier-less entries. SSC is in here today; you'll be extending its schema.
- `data/restaurants.json` — does not exist yet in the tree per my last check. When it gets created, tier-bearing entries live there.
- `index.html` — the rendered directory. Currently empty-state per the git log.
- `pitch-onepager.html` — the cold-outreach pitch sheet. Tier copy here will need a follow-up edit, but **do not edit it in this pass** — flag it as a follow-on so James sees it before the copy changes.

Note: `FOUNDING-MEMBERS.md`, `VISION-HANDOFF-FOR-MANUS.md`, and the entire `data/` directory are currently **untracked** in git. Commit before further edits land or you'll be working on top of unversioned ceremonial state.

---

## Schema change

Add a `coverage` object to the restaurant entry schema. This is declared coverage (what the restaurant says they filter), distinct from `readings` (what we measured) and `tier` (the rollup classification).

```typescript
{
  // existing fields preserved unchanged
  coverage: {
    drinking: "purified" | "tap" | "unknown",      // guest-facing drinking glass
    beverages: "purified" | "tap" | "unknown",     // bar, coffee, juice, soda fountain
    ice: "purified" | "tap" | "unknown",           // any ice served
    cooking: "purified" | "tap" | "unknown",       // kitchen prep, stocks, sauces
    whole_restaurant_poe: boolean                   // single POE filter for everything
  }
}
```

`unknown` is the default when a station hasn't been assessed (e.g., no reading at that tap and the owner didn't say).

Tier derivation becomes a pure function of coverage:

- `whole_restaurant_poe: true` → `Exemplary` (3 drops)
- Else, drinking + beverages + ice + cooking all `purified` → `Distinguished` (2 drops)
- Else, drinking + beverages both `purified` → `Recommended` (1 drop)
- Else → `null`

The `tier` field stays in the schema so existing code that reads it doesn't break, but **it should be computed from `coverage`, not hand-set**. If you want to enforce this, store it as a computed property at render time and remove it from the JSON entirely. Either way works; the constraint is that coverage is the source of truth and tier is derivable.

---

## Display logic

Listing template renders, in order:

1. **Founding badge** if `founding_member: true`. Always shown when present; independent of tier per the existing Founding Badge Rule.
2. **Tier badge** if tier is non-null. Drops icon per existing design.
3. **Coverage map** — always shown. Format suggestion (you decide the final UI):
   - "Drinking: purified · Beverages: tap · Ice: tap · Cooking: purified" for a partial-coverage listing
   - "Whole-restaurant purification (point of entry)" for an Exemplary listing
   - For unknown stations: render them or omit them — your call, but be consistent. I'd lean omit; "unknown" in a published receipt reads as a credibility hole.
4. **Readings** below the coverage map, per existing schema. The readings substantiate the coverage claim; the map is the human-readable summary.
5. **Filter system writeup** (existing field).
6. **Test date and verifier** (existing fields).

Empty-state page should still render when there are no entries with `publish_state: published` (or whatever the live flag becomes), but the rendering code must handle the transition cleanly when SSC's entry gets promoted post-kit-visit.

---

## Test case — Stars Shawarma City (post kit visit today)

After James completes the visit, SSC's entry will look approximately like:

```json
{
  "name": "Stars Shawarma City",
  "founding_member": true,
  "founding_status": { "confirmed_at": "2026-05-14", "is_inspiration_for_guide": true, ... },
  "coverage": {
    "drinking": "tap",
    "beverages": "tap",        // verify with James — Nikola may serve filtered behind the bar
    "ice": "tap",              // verify — ice machine line may be on the 3M
    "cooking": "purified",
    "whole_restaurant_poe": false
  },
  "tier": null,                  // derived; no tier earned
  "readings": [
    { "location": "drinking_station", "tds_ppm": 450, ... },   // example numbers; real values from today's visit
    { "location": "kitchen_prep", "tds_ppm": 80, ... },
    { "location": "bar", "tds_ppm": 440, ... },
    { "location": "ice_machine", "tds_ppm": 440, ... }
  ],
  "current_system": { "vendor": "3M", "stages": "two-stage carbon/sediment", "ro": false }
}
```

Render target: SSC's listing displays the founding badge, no tier badge, and a coverage map reading "Cooking: purified · Drinking: tap · Beverages: tap · Ice: tap" (assuming the worst case before kit visit confirms otherwise). The TDS readings sit underneath as the receipt. Owner narrative ("Nikola installed 3M after James asked about purified water...") forms the writeup section per FM#1's existing origin note.

**This is the listing that has to go live before Mad Hatter's industry night** (date TBD; James knows but I don't). The handout we hand out at that event drives traffic to theoasisguide.com — if the site is still empty-state at that point, the handout writes a check the site can't cash. SSC live = handout has a working example to point at.

---

## Hard constraints

- **Don't edit pitch-onepager.html or pitch-crib-sheet.md in this pass.** The tier copy in both ("Recommended is drinks/beverages, Distinguished adds cooking/ice...") is now subtly out of sync with the partial-coverage display reality. James needs to make the editorial call on whether the pitch copy needs revision or whether the disconnect is tolerable. Flag this as a follow-on; don't preempt.
- **Don't change the Founding Badge Rule.** It's already correctly worded — the founding badge is independent of tier, can stand alone, and can never be earned retroactively.
- **Don't promote FM#2/#3/#4 (Acqua di Mare / Mad Hatter Kava / Doug's Sidewinder) without agreement-date and origin-note from James.** They're staged at `sophia:~/shared/dispatch/oasis-guide/founding-additions-2026-05-17.md` with placeholders. The charter is append-only ceremonial state; don't write fiction into it.

---

## Verification when done

1. `pending-founding.json` schema validates against the updated structure (whatever validator you're using — JSON Schema, Zod, by-hand, doesn't matter).
2. SSC's entry renders correctly with the coverage map, no tier badge, founding badge present.
3. The empty-state page still renders when no entries are live yet.
4. A hypothetical Distinguished-tier entry (you can use a test fixture) renders with two drops + coverage map + tier badge.
5. Commit the previously-untracked files (`FOUNDING-MEMBERS.md`, `VISION-HANDOFF-FOR-MANUS.md`, `data/*`) before opening a PR or pushing to Pages.

---

## Open questions to confirm with James after the kit visit

- Does Nikola filter the **bar** drink line, the **ice machine** line, or both? Coverage map can't be honest without knowing. Ask which fixtures the 3M actually feeds.
- Same for **drinking station** — does SSC have a dedicated drinking-water tap that's separate from the kitchen-side filter? Sometimes the answer is "the dispenser is on the same line as the cooking sink and is therefore filtered." Sometimes not.
- Owner publish permission — Nikola gave verbal assent to the founding agreement, but the readings publication is a separate consent. Confirm explicit.
