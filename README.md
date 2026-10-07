# Orbit 971 — Satellite Monitoring of Agricultural Change

**Arab Youth Space Hackathon 2026 · Theme: Precision Agriculture & Crop Intelligence · Team Orbit 971 (UAE)**

## The problem

Farms in the UAE depend almost entirely on irrigation, and water is scarce and costly. A farm manager overseeing many fields has limited time to notice where crop conditions are changing, so problems are often found late, by walking the fields.

## Our solution and use case

Orbit 971 helps farm managers decide **where to review first**. Using free satellite data, it maps areas where vegetation greenness (NDVI) and moisture (NDMI) both declined, so the manager can check those areas against irrigation and crop records before changing anything.

- **User:** farm managers and agricultural operators overseeing multiple fields
- **Decision supported:** which areas to inspect first
- **Output:** dated change maps, data tables and a documented results package

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

Example outputs are committed in [`results/`](results/).

## Installation and running

### Option A — Google Colab (recommended)

1. Open `Orbit_971_15_Codes_new.ipynb` in [Google Colab](https://colab.research.google.com/) (File → Upload notebook, or open from GitHub)
2. Run the cells **in order**, from Code 01 to Code 15. Code 01 installs all dependencies:
   ```
   %pip install -q pystac-client planetary-computer rasterio scipy pandas matplotlib requests shapely
   ```
3. Code 06 and Code 07 take several minutes (imagery is read remotely)
4. Code 15 packages all outputs into a ZIP and downloads it

### Option B — Local Jupyter

```bash
git clone https://github.com/ramx-ra/orbit-971.git
cd orbit-971
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
pip install notebook
jupyter notebook Orbit_971_15_Codes_new.ipynb
```

When running locally: in Code 02, change `OUTPUT = Path("/content/orbit971_results_27km2")` to `OUTPUT = Path("orbit971_results_27km2")`, and skip Code 15 (Colab-only download). All results are saved in the output folder.

No account, API key or credentials are needed. All data are read from public catalogues.

## Input data

The notebook retrieves its input imagery automatically. The exact parameters are set in **Code 02**:

| Parameter | Value |
|---|---|
| Centre (lat, lon) | 24.730353, 54.982294 |
| Area | 5,400 m × 5,000 m (27 km²), UTM zone 40N (EPSG:32640) |
| Period | 2025-01-01 to 2025-12-31 |
| Collection | `sentinel-2-l2a` (Microsoft Planetary Computer) |
| Scene cloud filter | < 20% |
| Minimum local clear fraction | 0.80 |
| Grid resolution | 20 m |
| Thresholds | vegetation NDVI ≥ 0.30; NDVI and NDMI drop ≥ 0.10 |

The exact scene IDs used are listed in [`data/selected_scenes.csv`](data/).

## Repository structure

```
README.md                       Project description and instructions
requirements.txt                Pinned Python packages
Orbit_971_15_Codes_new.ipynb    Main notebook (with saved outputs)
data/                           Sample input: scene lists used in the analysis
results/                        Example outputs: maps, tables, manifest
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
