# iNat-Mapping

Project to generate cumulative observation frames and GIFs from iNaturalist and other observation sources.

**Overview**
- This repository contains data, processing notebooks, and generated output frames for mapping observation accumulation over time.
- The main runnable artifact at the repo root is the notebook: [observations_conus_gif.ipynb](observations_conus_gif.ipynb).

**Repository structure**
- `data/`: raw input CSVs organized by taxon and source (example: `amanita_phalloides/iNat/observations-663867.csv`).
- `outputs/`: generated visualization output. Each taxon has subfolders with `frames/` containing `cum_YYYY.html` files and other artifacts.
- `observations_conus_gif.ipynb`: primary notebook that loads data, produces per-year cumulative maps/frames, and (optionally) assembles GIFs.

What this README covers:
- How the code at the repo root works.
- What you must do manually to provide data paths.
- Minimal steps to run the notebook and produce outputs.

## How the root notebook works
- The notebook loads CSV observation files from `data/`, filters/aggregates observations by date and location, and creates an incremental set of map frames (one per year) which are written into `outputs/<taxon>/frames/`.
- The notebook is designed to be semi-configurable: you set the input data paths and the output directory at the top of the notebook before running.

## Manual data setup (required)
The repository does not auto-download observation files. You must provide CSVs in `data/` (or point the notebook to where you keep them). Typical workflow:

1. Collect or place your observation CSV files into a folder under `data/`, maintaining a simple structure such as:

   - `data/<taxon_name>/iNat/observations-XXXXXX.csv`
   - `data/<taxon_name>/mushroom_observer_data/observations_XXX.csv`

2. Open [observations_conus_gif.ipynb](observations_conus_gif.ipynb) and edit the configuration variables near the top. Example variables to set:

```python
# Example configuration at top of notebook
DATA_ROOT = '/absolute/path/to/repo/data'  # or relative path 'data'
OUTPUT_ROOT = '/absolute/path/to/repo/outputs'  # or relative path 'outputs'
TAXON = 'amanita_phalloides'  # subfolder name under data
SOURCES = ['iNat', 'mushroom_observer_data']  # subfolders to include
YEAR_RANGE = (2000, 2026)  # range of years for frames
```

3. Ensure the CSV filenames and columns expected by the notebook exist (date, latitude, longitude, possibly `source`/`taxon` columns). If your CSVs use different column names, edit the data-loading cell to map your column names.

## Running the notebook
Prerequisites (suggested)
- Python 3.8+ and a working Jupyter environment.
- Typical Python packages used by the notebook: `pandas`, `geopandas` (optional), `folium` or `altair` (depending on visualization), `imageio` (if making GIFs), and `notebook`/`jupyterlab`.

Minimal run steps

1. Create a virtual environment and install packages (example):

```bash
python -m venv .venv
source .venv/bin/activate
pip install pandas geopandas folium imageio jupyter
```

2. Start Jupyter and open the notebook:

```bash
jupyter notebook observations_conus_gif.ipynb
```

3. In the notebook, confirm `DATA_ROOT`, `OUTPUT_ROOT`, and `TAXON` are set to point to your CSVs and where you want outputs written. Run cells in order.

4. Inspect generated frames in `outputs/<taxon>/frames/`.

## Making this easier (optional)
- I can add a `requirements.txt` or `environment.yml` listing exact package versions used by the notebook. Let me know if you want that.
- If you prefer, I can extract the config variables into a small `config.py` or JSON file and update the notebook to read from it.

## Notes and next steps
- The notebook assumes manual control over data placement and basic familiarity with Jupyter notebooks.
- If you want, I can:
  - generate `requirements.txt` for the notebook, or
  - refactor the notebook to read a single `config.json` and accept env vars for paths, or
  - add a small CLI script to run the pipeline outside Jupyter.

If you'd like any of those, tell me which and I'll implement it.
