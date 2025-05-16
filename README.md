# MELTNET Patch Extraction and Antarctic Melt Rate Calculation

This repository provides a workflow to extract geospatial patches from Antarctic ice shelf regions and compute basal melt rates using oceanographic and topographic datasets. The code is designed to run in a Jupyter Notebook environment and automates data download, preprocessing, patch extraction, and melt rate calculation.

---

## **Environment Setup**

This project requires Python 3.9+ and is best run in a [conda](https://docs.conda.io/en/latest/) environment, especially for geospatial dependencies.

### **Required Dependencies**

- `numpy >= 1.22`
- `xarray >= 2023.1`
- `scipy >= 1.10`
- `matplotlib >= 3.5`
- `torch >= 2.0`
- `scikit-learn >= 1.2`
- `pyproj >= 3.4`
- `earthaccess >= 0.5`
- `cartopy >= 0.21` (optional, for geographic visualizations-**install via conda is preferred**)

## **Dataset Requirements**

- **BedMachine Antarctica v3** (NSIDC-0756): Antarctic bed topography and ice thickness (NetCDF).
- **ECCO L4 Temperature & Salinity Monthly Climatology** (ECCO_L4_TEMP_SALINITY_05DEG_MONTHLY_V4R4): Ocean temperature and salinity (NetCDF).

The notebook automates searching and downloading these datasets using the [earthaccess](https://github.com/nsidc/earthaccess) library. You will need a [NASA Earthdata Login](https://urs.earthdata.nasa.gov/) account.

---

## **How to Run**

### **1. Download Data**

- Open the notebook `MSML605_MELTNET.ipynb` in Jupyter.
- Run all cells in order.
- When prompted, enter your Earthdata Login credentials.
- The notebook will:
  - Search and download the BedMachine Antarctica v3 file.
  - Download ECCO monthly temperature and salinity files for 2014–2016.

**Downloaded files will be saved in:**
- BedMachine: current directory (`./`)
- ECCO: `./data/`

### **2. Patch Extraction and Processing**

- The notebook will:
  - Clip a region of interest from BedMachine.
  - Interpolate ECCO temperature and salinity onto the BedMachine grid.
  - Extract overlapping 64×64 patches (stride 16) with bed elevation, ice draft, temperature, and salinity.
  - Compute local freezing point and thermal forcing.
  - Calculate melt rates using a quadratic-local parameterization.
  - Save results as `meltnet_input_patches.npy` and `meltnet_melt_targets.npy`.

### **3. Visualization**

- The notebook provides an example visualization of a patch and melt rate at the end.

---

## **Outputs**

- `meltnet_input_patches.npy`: Input features for machine learning (shape: [N, 64, 64, 4])
- `meltnet_melt_targets.npy`: Corresponding melt rate targets (shape: [N, 64, 64])

---

## **Troubleshooting**

- **Cartopy installation issues:** Use `conda install -c conda-forge cartopy pyproj`.
- **Earthaccess authentication:** Ensure your Earthdata Login is active and authorized.
- **Memory errors:** Patch extraction may require significant RAM; reduce region size or patch count if needed.



