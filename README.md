# Orbit 971 — Satellite Monitoring of Agricultural Change

**Arab Youth Space Hackathon 2026 · Theme: Precision Agriculture & Crop Intelligence · Team Orbit 971 (UAE)**

Orbit 971 helps farm managers decide **where to review first**. It maps areas where vegetation greenness and moisture indices both declined, so managers can check those areas against irrigation and crop records before changing anything.

## What it does

- Searches 2025 **Sentinel-2 L2A** imagery (Microsoft Planetary Computer) for a 5.4 km × 5 km (27 km²) area in Abu Dhabi
- Screens cloud and shadow per pixel and selects one clear snapshot per month (12 in total)
- Calculates **NDVI, NDRE, NDMI and BSI** on a common 20 m grid
- Flags pixels where **NDVI and NDMI both fell by ≥ 0.10** compared with earlier 2025 snapshots
- Aligns **Landsat-9 surface temperature** (1-day gap) as extra context
- Inventories **EnMAP** hyperspectral coverage (discovery only, no pixels used)
- Exports GeoTIFF maps, CSV tables, figures and a project manifest

## Results (latest snapshot: 3 Dec 2025)

| Item | Value |
|---|---|
| Vegetation area assessed | 1,083.96 ha |
| Area flagged for review | 106.08 ha (9.79%) |
| Landsat-9 valid thermal coverage | 99.8% |

Threshold sensitivity: 0.05 → 22.63% · 0.10 → 9.79% · 0.15 → 2.96%

## How to run

1. Open `Orbit_971_14_Codes_new.ipynb` in **Google Colab**
2. Run the cells **in order**, from Code 01 to Code 15
3. Code 06 and Code 07 take several minutes (they read imagery remotely)
4. Code 15 packages all outputs into a ZIP and downloads it

No account, API key or credentials are needed. All data are read from public catalogues.
Package versions are pinned in `requirements.txt`.

## Repository contents

```
Orbit_971_14_Codes_new.ipynb   Main notebook (with saved outputs)
requirements.txt               Pinned Python packages
examples/                      Example outputs (maps, tables, manifest)
README.md                      This file
```

## Limitations

- Flags show **change for review**, not confirmed water stress or its cause
- No field validation has been done; accuracy is not measured
- The comparison is not seasonally matched; harvest and replanting can trigger flags
- Thresholds are exploratory, not calibrated crop limits
- The study rectangle is not a verified farm boundary; 20 m pixels may mix plants, soil and structures
- No irrigation volumes or water savings are estimated

## Data sources

- Copernicus Sentinel-2 L2A — via Microsoft Planetary Computer
- USGS Landsat Collection 2 Level-2 (Landsat-9 surface temperature) — via Microsoft Planetary Computer
- EnMAP L2A catalogue — DLR (metadata only)
