# NOTE
This project is intended to be hosted on Netlify Free Tier. All data sources, APIs, libraries, and dependencies must be free to use for commercial purposes and compatible with static frontend deployment (no required backend infrastructure).

The platform will be designed as a lightweight, client-side application optimised for Netlify hosting.

Future monetisation will include advertising placements in the left and right margins of the UI once sufficient user traffic is achieved.

Before implementing monetisation, we must integrate basic user analytics (e.g. page views, unique users, session tracking) using a free analytics solution compatible with Netlify (e.g. Netlify Analytics or equivalent privacy-friendly alternative) to measure adoption and usage trends.

## Riverline – High-Accuracy Flood Risk Engine (Revised Spec)

## Objective
Upgrade Riverline from a simple flood screening tool into a multi-factor flood risk intelligence system while preserving the existing UI and lightweight performance.

The goal is to maximise practical flood risk accuracy using layered geospatial datasets, not full hydrodynamic simulation.

---

# Core System Architecture

Flood risk is computed from a weighted combination of:

- River network proximity (OpenStreetMap)
- Rainfall climatology + extreme rainfall events
- Terrain elevation (30m DEM)
- Slope + flow accumulation (derived)
- Soil permeability / infiltration
- Urbanisation / land use
- Climate change projections (RCP scenarios)

Final output:
→ Single unified Flood Risk Score + Tier (Low / Moderate / High / Severe)

---

# 1. River Network Integration (OpenStreetMap)

## Data Source
- OpenStreetMap waterways (rivers, streams)

## Purpose
- Define hydrological structure and drainage paths

## Outputs
- Distance to nearest river
- River density within catchment radius
- River proximity risk factor

---

# 2. Rainfall Layer Integration

## Data Sources
- Average annual rainfall (climatology datasets)
- Historical rainfall extremes / event frequency datasets

## Purpose
- Estimate water input forcing into system

## Outputs
- Baseline rainfall intensity (annual)
- Extreme rainfall probability factor
- Storm event amplification risk

---

# 3. DEM Terrain Model (30m Resolution)

## Data Source
- Existing 30m DEM files (from Three.js MVC project)

## Purpose
- Primary physical driver of flood susceptibility

## Outputs
- Elevation at point
- Local slope gradient
- Low-lying terrain detection
- Flow direction approximation (simplified)

---

# 4. Derived Terrain Hydrology (From DEM)

## Computed Layers
- Slope classification
- Flow accumulation approximation
- Depression / pooling likelihood zones

## Purpose
- Identify where water naturally concentrates

---

# 5. Soil & Permeability Layer

## Data Source
- Global soil datasets (e.g. SoilGrids / FAO)

## Purpose
- Controls infiltration vs surface runoff

## Outputs
- High runoff soils → increased flood risk
- High permeability soils → reduced flood risk
- Soil infiltration factor (0–1)

---

# 6. Urbanisation / Land Use Layer

## Data Source
- OpenStreetMap land use + built-up classification

## Purpose
- Quantify impervious surface impact

## Outputs
- Urban density factor
- Impervious surface proxy
- Drainage disruption index

---

# 7. Climate Change / RCP Integration

## Framework
- IPCC RCP scenarios (2.6 / 4.5 / 8.5)

## Purpose
- Adjust future flood risk expectations

## Outputs
- Climate risk multiplier
- Scenario toggle:
  - Current climate
  - Medium emissions
  - High emissions

---

# 8. Unified Flood Risk Engine

## Method
Each layer produces a normalized score (0–1):

- River proximity factor
- Rainfall factor
- Elevation factor
- Slope factor
- Soil factor
- Urbanisation factor
- Climate factor

## Final Model

Flood Risk Score = weighted sum of all factors

## Output Classification
- Low
- Moderate
- High
- Severe

---

# 9. UI Constraints (Critical)

The existing Riverline UI must remain unchanged:

- Search system remains identical
- Map layout unchanged (Leaflet)
- Result card structure unchanged

Only backend logic and interpretation layers are enhanced.

---

# 10. Performance Constraints

System must remain:

- Lightweight
- Browser-friendly
- Fast (<2–3 seconds response target)

No full hydrodynamic simulation or 3D computation in this product.

---

# Core Design Principle

This system is not a physics simulator.

It is a multi-layer geospatial risk intelligence engine that combines:

- Terrain (DEM)
- Hydrology (rivers + flow proxies)
- Climate (rainfall + RCP)
- Land use (urbanisation)
- Soil properties

Into a single, interpretable flood risk output for early-stage decision making. Riverline – High-Accuracy Flood Risk Engine (Revised Spec)

## Objective
Upgrade Riverline from a simple flood screening tool into a multi-factor flood risk intelligence system while preserving the existing UI and lightweight performance.

The goal is to maximise practical flood risk accuracy using layered geospatial datasets, not full hydrodynamic simulation.

---

# Core System Architecture

Flood risk is computed from a weighted combination of:

- River network proximity (OpenStreetMap)
- Rainfall climatology + extreme rainfall events
- Terrain elevation (30m DEM)
- Slope + flow accumulation (derived)
- Soil permeability / infiltration
- Urbanisation / land use
- Climate change projections (RCP scenarios)

Final output:
→ Single unified Flood Risk Score + Tier (Low / Moderate / High / Severe)

---

# 1. River Network Integration (OpenStreetMap)

## Data Source
- OpenStreetMap waterways (rivers, streams)

## Purpose
- Define hydrological structure and drainage paths

## Outputs
- Distance to nearest river
- River density within catchment radius
- River proximity risk factor

---

# 2. Rainfall Layer Integration

## Data Sources
- Average annual rainfall (climatology datasets)
- Historical rainfall extremes / event frequency datasets

## Purpose
- Estimate water input forcing into system

## Outputs
- Baseline rainfall intensity (annual)
- Extreme rainfall probability factor
- Storm event amplification risk

---

# 3. DEM Terrain Model (30m Resolution)

## Data Source
- Existing 30m DEM files (from Three.js MVC project)

## Purpose
- Primary physical driver of flood susceptibility

## Outputs
- Elevation at point
- Local slope gradient
- Low-lying terrain detection
- Flow direction approximation (simplified)

---

# 4. Derived Terrain Hydrology (From DEM)

## Computed Layers
- Slope classification
- Flow accumulation approximation
- Depression / pooling likelihood zones

## Purpose
- Identify where water naturally concentrates

---

# 5. Soil & Permeability Layer

## Data Source
- Global soil datasets (e.g. SoilGrids / FAO)

## Purpose
- Controls infiltration vs surface runoff

## Outputs
- High runoff soils → increased flood risk
- High permeability soils → reduced flood risk
- Soil infiltration factor (0–1)

---

# 6. Urbanisation / Land Use Layer

## Data Source
- OpenStreetMap land use + built-up classification

## Purpose
- Quantify impervious surface impact

## Outputs
- Urban density factor
- Impervious surface proxy
- Drainage disruption index

---

# 7. Climate Change / RCP Integration

## Framework
- IPCC RCP scenarios (2.6 / 4.5 / 8.5)

## Purpose
- Adjust future flood risk expectations

## Outputs
- Climate risk multiplier
- Scenario toggle:
  - Current climate
  - Medium emissions
  - High emissions

---

# 8. Unified Flood Risk Engine

## Method
Each layer produces a normalized score (0–1):

- River proximity factor
- Rainfall factor
- Elevation factor
- Slope factor
- Soil factor
- Urbanisation factor
- Climate factor

## Final Model

Flood Risk Score = weighted sum of all factors

## Output Classification
- Low
- Moderate
- High
- Severe

---

# 9. UI Constraints (Critical)

The existing Riverline UI must remain unchanged:

- Search system remains identical
- Map layout unchanged (Leaflet)
- Result card structure unchanged

Only backend logic and interpretation layers are enhanced.

---

# 10. Performance Constraints

System must remain:

- Lightweight
- Browser-friendly
- Fast (<2–3 seconds response target)

No full hydrodynamic simulation or 3D computation in this product.

---

# Core Design Principle

This system is not a physics simulator.

It is a multi-layer geospatial risk intelligence engine that combines:

- Terrain (DEM)
- Hydrology (rivers + flow proxies)
- Climate (rainfall + RCP)
- Land use (urbanisation)
- Soil properties

Into a single, interpretable flood risk output for early-stage decision making.
