**The licensing problem that affects everything.** Riverline's flood layer (and any new rainfall/climate layer) runs on Open-Meteo, which is explicitly free only for non-commercial use. Their terms define commercial use to include operating websites or apps that have subscriptions or display advertisements, and they reserve the right to block applications and IP addresses that misuse the service. So the moment margin ads go live, continued use of the free Open-Meteo endpoints would violate their terms — you'd need their paid commercial/customer-API tier at that point. The good news: your own sequencing (analytics first, ads later) already buys you time, since private or non-profit websites or apps that do not have subscriptions or advertising qualify as non-commercial. Practical takeaway: budget for an Open-Meteo commercial plan as a line item the moment ads turn on, don't treat "free" as permanent.

**Per-layer reality check**

River network (OSM/Overpass) — already working today. Extending it to "river density in catchment radius" is a straightforward query change, no new risk.

Rainfall climatology + extremes — genuinely buildable now. Open-Meteo's historical weather archive (ERA5, back to 1940, same free/non-commercial terms as the flood API) gives real daily precipitation, so annual climatology and extreme-event frequency can be computed client-side from one more fetch.

Elevation — buildable now via Open-Meteo's free Elevation API, point-based, real data.

Slope / flow accumulation — this is where the spec overpromises. A slope estimate is doable by sampling elevation at a small ring of points around the target and taking the gradient — coarse, but real. True flow accumulation (water concentrating across an upstream catchment) requires actual raster DEM processing, the kind QGIS or richdem does — it cannot be derived from a handful of point queries, and there's no free point API for it. That part either gets dropped, downgraded to the slope proxy, or requires a precomputed tile pipeline baked in at build time, which is a much bigger undertaking than "add an API call."

Soil permeability — ISRIC SoilGrids has a real public query API, but I haven't verified it returns CORS headers for direct browser fetch (a lot of geospatial WCS-style services don't, which would silently break a static-only deployment). This needs a 10-minute spike test before it's trusted in the plan.

Urbanisation / land use — buildable now, same Overpass pattern already in the codebase (landuse/building density within a radius).

Climate / RCP — Open-Meteo has a real Climate API (CMIP6, 10km downscaled, 1950–2050), same non-commercial terms as above. But it doesn't give a clean RCP2.6/4.5/8.5 selector — it's a multi-model ensemble that's as close to RCP8.5 as possible within CMIP6, and projections beyond 2050 aren't part of this API. So "current vs. medium vs. high emissions toggle" isn't honestly deliverable as written — what's deliverable is "today's climate normal vs. the 2050 ensemble projection for this location."

**Weighting and the "high-accuracy" name.** The weighted-sum architecture itself is fine, but the weights are heuristic, not calibrated against actual flood-event outcomes — so it's a transparent multi-signal screening heuristic, not a validated accuracy model. I'd carry the same plain-language disclaimer pattern already in the result card into this, and possibly soften "High-Accuracy" in any public-facing copy.

**Hosting and analytics.** Netlify's free tier is fine at low traffic but now runs on a credit system (deploys, bandwidth, functions all draw down a monthly credit pool), worth watching once ad-driven traffic grows. "Netlify Analytics" is a paid add-on, not free — Cloudflare Web Analytics is the better fit for the brief: free, cookieless, no consent banner needed, just one script tag, works regardless of who hosts the site. That can go in now, ahead of ads, with zero conflict.

**Proposed phasing**
Phase 1: rainfall climatology/extremes + land-use density — both real, low-risk, slot into the existing result card with no UI change. Add Cloudflare Web Analytics here too.
Phase 2: elevation + slope proxy, soil layer (pending the SoilGrids CORS check).
Phase 3: climate outlook (current vs. 2050 CMIP6), framed honestly rather than as an RCP toggle.
Parked: true flow accumulation / depression modeling, until you're open to a build-time data pipeline rather than pure runtime API calls.
Gate: before ads go live, move flood + rainfall + climate calls to Open-Meteo's commercial tier.

One performance note: stacking 6–7 third-party calls per search (flood, river name, rainfall, land use, elevation, soil, climate) makes the stated 2–3 second target tight if done sequentially — they should run in parallel (`Promise.all`), and even then treat the target as best-effort given Overpass alone can be slow under load.

