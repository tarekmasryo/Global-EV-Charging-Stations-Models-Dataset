# ⚡ Global EV Charging Stations (2025)  
**Author:** [Tarek Masryo](https://github.com/tarekmasryo) · [Kaggle](https://www.kaggle.com/datasets/tarekmasryo/global-ev-charging-stations)  
**Version:** v1.0 (2025-09-15)  

**License:**  
• Charging data (Open Charge Map) — **CC BY 4.0**  
• Companion file `ev_models_2025.csv` — **CC0**  

---

## TL;DR

> Global EV charging snapshot: **242,417 sites** across **121 countries**, with **clean, standardized columns**.  
>  
> - Main global file: `charging_stations_2025_world.csv`  
> - ML-ready file: `charging_stations_2025_ml.csv`  
> - Summaries: `country_summary_2025.csv`, `world_summary_2025.csv`  
> - Companion EV models: `ev_models_2025.csv`  

---

## Why this dataset exists
EV charging infrastructure is often fragmented and inconsistent. This dataset delivers a **single, analysis-ready bundle** to explore site coverage, power levels, and fast-DC availability worldwide.

---

## What’s inside

### Main file — `charging_stations_2025_world.csv`  
One row per site (11 columns):  

- `id` (int): OCM unique site ID  
- `name` (str): site name  
- `city` (str)  
- `country_code` (ISO-2)  
- `state_province` (str)  
- `latitude`, `longitude` (float, WGS84)  
- `ports` (int): number of charging points  
- `power_kw` (float): max power (kW)  
- `power_class` (str): derived category from `power_kw`  
- `is_fast_dc` (bool): `true` if `power_kw ≥ 50`  

### ML-ready file — `charging_stations_2025_ml.csv`  
Compact version (7 columns), deduplicated and simplified for direct machine learning use.  

### Helper files
- `country_summary_2025.csv` — per-country counts & max power  
- `world_summary_2025.csv` — global roll-up  

### Companion file
- `ev_models_2025.csv` — EV models (make, model, regions, powertrain, year, body style, origin)  

---

## How it was built
- Collected per-country via the Open Charge Map API (`/v3/poi`)  
- Deduplicated by unique `id`  
- Dropped invalid coordinates, validated ranges  
- Derived fields: `power_class`, `is_fast_dc`  
- Generated country and world summaries for quick analysis  

---

## License & attribution
- **Charging data**: Open Charge Map (openchargemap.org) — **CC BY 4.0**  
  Must credit: **“Contains data © Open Charge Map contributors.”**  
- **EV models file** (`ev_models_2025.csv`): released under **CC0** (no attribution required).  

---

## Quick start (pandas)
```python
import pandas as pd

df = pd.read_csv("charging_stations_2025_world.csv")
print(df.shape)

# Top 15 countries by number of sites
top15 = (df.groupby("country_code")["id"].count()
           .sort_values(ascending=False).head(15))
print(top15)

# Share of fast-DC sites (≥50 kW)
fast_dc_share = (df["is_fast_dc"]).mean()
print("Fast-DC share:", round(fast_dc_share, 3))
