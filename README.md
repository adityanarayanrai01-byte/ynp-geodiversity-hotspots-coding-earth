# Coding Earth in practice: A fully reproducible Python workflow for 1-km geodiversity hotspot mapping in Yellowstone National Park

This repository contains the **code + documentation** for an open-source, reproducible workflow that maps **1-km geodiversity hotspots** in **Yellowstone National Park (YNP)** from three interpretable abiotic components:

- **Thermal footprint** (per-cell thermal area, m²)
- **Lithological richness** (unique geologic units per cell)
- **Terrain heterogeneity** (standard deviation of slope, degrees)

These components are min–max normalized and combined into a composite **geodiversity index**, and **hotspots** are delineated using a **p90 rule** (top 10% by index).

---

## Full replication bundle (data + code) — Zenodo

⚠️ **Important:** This GitHub repository is intentionally lightweight and does **NOT** include large processed inputs (e.g., rasters, full GPKGs).  
For a self-contained, citable replication package (processed inputs where licensing permits, manifests, checksums, scripts, and expected outputs), use the Zenodo archive:

- **Zenodo DOI:** 10.5281/zenodo.18519435  
- **Record:** https://zenodo.org/records/18519435

The recommended reproduction path is:
1) download the Zenodo ZIP, 2) unzip locally, 3) run the one-click script.

---

## How to reproduce (recommended)

### 1) Set up the Python environment
Use either conda or pip.

**Option A — Conda**
```bash
conda env create -f environment.yml
conda activate ynp-codingearth
