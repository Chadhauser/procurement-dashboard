# Bid Radar — verdict test set

Run each tender text through the **Check a tender** tab (or POST it to `/functions/v1/assess` as `{ "text": "..." }`). All are English councils, closing in ~20 working days.

| # | Tender text | Expected verdict |
|---|---|---|
| A | Project management support for a leisure centre refurbishment programme. £60,000 over 9 months. | **BID - WE DELIVER** |
| B | Grounds maintenance for parish open spaces. £40,000. *With a grounds subcontractor named in settings.* | **BID AS PRIME** |
| C | Same as B, subcontractor set to "none yet". | **WATCH** |
| D | Programme and contract management office for highways capital programme. £450,000. | **APPROACH AS SUB** (our lane, over the £90k cap) |
| E | Supply of school minibuses. £70,000. | **NO-BID** (outside our lane) |

## Last verified — 2 Oct 2026
- A → BID - WE DELIVER, C → WATCH, D → APPROACH AS SUB, E → NO-BID — confirmed live through the `assess` engine.
- B → BID AS PRIME — this comes from the radar's **Needs a sub** display: a grounds-category tender shows "Watch" while the subcontractor is "none yet" and "Bid as prime" once a subcontractor name is set in `CFG.subCategories`. The AI engine returns WATCH for a grounds tender with no sub named (correct); the "Bid as prime" step is the named-sub case.

## Notes
- Both engines (`assess` for Check-a-tender, `bid-triage` for the "Should we bid?" button) share the same verdict decision order: outside-lane → NO-BID; in-lane & over cap → APPROACH AS SUB; subcontract category → WATCH / BID AS PRIME; otherwise BID - WE DELIVER.
- Re-run this set after any change to the verdict prompts.
