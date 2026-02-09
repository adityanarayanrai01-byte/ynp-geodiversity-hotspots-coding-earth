

````markdown
# Coding Earth in practice: A fully reproducible Python workflow for 1-km geodiversity hotspot mapping in Yellowstone National Park

This repository contains the **code + documentation** for an open-source, reproducible workflow that maps **1-km geodiversity hotspots** in **Yellowstone National Park (YNP)** from three interpretable abiotic components:

- **Thermal footprint** (per-cell thermal area, m²)
- **Lithological richness** (unique geologic units per cell; optional sliver control)
- **Terrain heterogeneity** (standard deviation of slope, degrees)

These components are min–max normalized and combined into a composite **geodiversity index (GDI)**, and **hotspots** are delineated using a **p90 rule** (top 10% by index).

---

## Full replication bundle (data + code) — Zenodo

⚠️ **Important:** This GitHub repository is intentionally lightweight and does **NOT** include large processed inputs (e.g., rasters, full GPKGs).  
For a **self-contained, citable replication package** (scripts + processed artifacts + manifests + checksums + expected outputs), use the Zenodo archive:

- **Zenodo DOI (canonical replication package):**
  `https://doi.org/10.5281/zenodo.18543110`
- **Record page:**
  `https://zenodo.org/records/18543110`

**Recommended reproduction path:**  
1) download the Zenodo ZIP, 2) unzip locally, 3) verify integrity, 4) run the minimal script sequence below.

---

## Quick start (recommended: Zenodo bundle)

> Run commands **from the unzipped bundle root** (the folder that contains `scripts/`, `docs/`, `requirements.txt`, `SHA256SUMS_ALL.txt`, etc.).

### 1) Verify integrity
Linux/macOS:
```bash
sha256sum -c SHA256SUMS_ALL.txt
````

Windows (PowerShell):

```powershell
# If sha256sum isn't available, you can still validate by comparing hashes
# using Get-FileHash, but the bundle is designed for sha256sum -c.
```

### 2) Create an environment and install dependencies

**Option A — Conda (recommended)**

```bash
conda create -n ynp_geodiv python=3.11
conda activate ynp_geodiv
pip install -r requirements.txt --upgrade
```

**Option B — pip/venv**

```bash
python -m venv .venv
source .venv/bin/activate   # macOS/Linux
# .\.venv\Scripts\activate  # Windows PowerShell
pip install -r requirements.txt --upgrade
```

---

## How to reproduce (minimal, OS-agnostic)

### Default rerun (sliver-controlled geology richness; MIN_GEO_IX_AREA_M2 = 10,000 m²)

Linux/macOS:

```bash
export MIN_GEO_IX_AREA_M2=10000
python scripts/02_core_build_obj1_grid_thermal_geology_slopeSD.py
python scripts/07_export_obj1_p90_summary.py
python scripts/08_sliver_diagnostics_geo_overlay.py
python scripts/04_make_fig1_maps_scalebar_outside.py
```

Windows (PowerShell):

```powershell
$env:MIN_GEO_IX_AREA_M2="10000"
python scripts/02_core_build_obj1_grid_thermal_geology_slopeSD.py
python scripts/07_export_obj1_p90_summary.py
python scripts/08_sliver_diagnostics_geo_overlay.py
python scripts/04_make_fig1_maps_scalebar_outside.py
```

This regenerates:

* the Objective-1 hotspot GPKG,
* p90 summary outputs,
* sliver diagnostics (10,000 m²),
* Figure 1 exports (PNG + hi-res PNG + PDF).

### Sensitivity rerun (no sliver filtering; MIN_GEO_IX_AREA_M2 = 0)

> Note: This run overwrites `processed_obj1/`. Copy/rename that folder if you want to keep the default outputs.

Linux/macOS:

```bash
export MIN_GEO_IX_AREA_M2=0
python scripts/02_core_build_obj1_grid_thermal_geology_slopeSD.py
python scripts/07_export_obj1_p90_summary.py
python scripts/04_make_fig1_maps_scalebar_outside.py
```

Windows (PowerShell):

```powershell
$env:MIN_GEO_IX_AREA_M2="0"
python scripts/02_core_build_obj1_grid_thermal_geology_slopeSD.py
python scripts/07_export_obj1_p90_summary.py
python scripts/04_make_fig1_maps_scalebar_outside.py
```

### Figure 1 only (optional)

```bash
python scripts/04_make_fig1_maps_scalebar_outside.py
```

---

## Expected outputs

After a successful run, confirm the following exist in `processed_obj1/`:

* `geodiversity_hotspots_obj1_slopeSD.gpkg`
  **Layer:** `obj1_grid_hotspots`
  **Key fields:** `thermal_area_m2`, `geology_richness`, `slope_sd_deg`, min–max normals, `geodiv_index`, `hotspot_p90`
* `obj1_hotspot_p90_summary.csv`
* `hotspot_summary_obj1_slopeSD.csv`
* `sliver_diagnostics_10000m2.csv` (default run)
* Figure 1 exports:

  * `Fig1_Obj1_Hotspots_FINAL_STRIP.png`
  * `Fig1_Obj1_Hotspots_FINAL_STRIP_HIRES.png`
  * `Fig1_Obj1_Hotspots_FINAL_STRIP.pdf`

---

## One-click runners (available, but not the “minimal” path)

Two convenience wrappers exist:

* `scripts/00_run_all_obj1_sliver10000.py`
* `scripts/00_run_all_obj1_nosliver.py`

They are useful for quick reruns, but for a fully documented “minimal reproducibility” path (including the exact summary/diagnostics files above), use the **minimal script sequence** in this README.

---

## Parameters (Objective 1)

Sensitivity tests are supported via environment variables and single-line constants:

* `MIN_GEO_IX_AREA_M2` (default **10000**; environment variable): sliver control for grid×geology intersections. Set to **0** to disable.
* Hotspot percentile (default **0.90**; constant in code): hotspots defined as GDI ≥ p90.
* Grid resolution (default **1000 m**; constant in code): `cell = 1000.0`. Changing it requires rebuilding the grid and recomputing all components.
* Zonal stats pixel rule (default **all_touched=False**; constant in code): retained for stability across minor raster alignment differences.

---

## Data sources (core)

* Park boundary / AOI: NPS Land Resources Division Boundary & Tract Data Service (FeatureServer)
* DEM: USGS 3DEP 1 arc-second (~30 m) (The National Map/3DEP)
* Thermal areas: “Map of Yellowstone’s Thermal Areas: Updated 2023-12-31” (USGS data release; see manuscript/Zendo record)
* Geology: USGS Open-File Report 99-174 digital geologic map (Christiansen & Wahl, 1999)
* Gazetteer names (optional): USGS GNIS (The National Map)

---

## How to cite

If you use this workflow or its outputs, cite the archived replication package:

* Zenodo DOI: `https://doi.org/10.5281/zenodo.18543110`

Recommended dataset citation (geology):

* Christiansen RL & Wahl RR (1999). *Digital geologic map of Yellowstone National Park and vicinity.* USGS Open-File Report 99-174. `https://doi.org/10.3133/ofr99174`

---

## License

* Code: MIT (see `LICENSE` if included in this repository / Zenodo bundle)
* Data and derived artifacts: follow original data-provider licenses; see Zenodo record metadata.

---

## Troubleshooting

When opening an issue, include:

* OS + Python version
* the exact command you ran
* full traceback
* `VERSION.txt` (from the Zenodo bundle)

```

If you paste your **current full README** (the rest after “Option A — Conda”), I can **surgically edit** it instead of replacing the whole thing—keeping your sections but updating only what’s necessary.
```
