# TL;DR

> A clean, single-file global snapshot of EV charging sites: **242,417 rows** across **121 countries**, with **11 tidy columns**.  
>  
> - One global CSV (`charging_stations_2025_world.csv`) deduplicated by `id`  
> - Optional roll-ups (`country_summary_2025.csv`, `world_summary_2025.csv`)  
> - Plus an **EV models** companion file (`ev_models_2025.csv`) for context

---

## Why this dataset exists
Electric mobility is booming, but analysis-ready data is scattered, inconsistent, and full of edge cases. This project aims to be a practical bridge: a **human-friendly, single CSV** you can load in seconds to explore coverage, power levels, and fast-DC availability worldwide. It’s a global snapshot pulled on **2025-09-01**.

---

## What’s inside
**Main file — `charging_stations_2025_world.csv` (one row per site, global)**

**Columns (11):**
- `id` (int): OCM unique site ID  
- `name` (str): site name  
- `city` (str)  
- `country_code` (ISO-2)  
- `state_province` (str)  
- `latitude`, `longitude` (float, WGS84)  
- `ports` (int): number of charging points at the site  
- `power_kw` (float): maximum power (kW) among listed connectors  
- `power_class` (str): derived from `power_kw` thresholds  
- `is_fast_dc` (bool): `true` if `power_kw ≥ 50`  

**Helper files:**
- `country_summary_2025.csv` — per-country counts and max power  
- `world_summary_2025.csv` — quick global roll-up  

**Companion file:**
- `ev_models_2025.csv` — EV model records (useful for context, modeling, and joinable analyses)

---

## How it was built
- Pulled per-country sites via the Open Charge Map API  
- Removed duplicates by `id` and rows without coordinates  
- Derived `power_class` and `is_fast_dc` from `power_kw`  
- Merged everything into one global CSV; generated compact summaries  
- Clean-up note (Sept 2025): ensured consistency across files → Namibia (`NA`) added, Saint Lucia (`LC`) removed → final scope = **121 countries**  
- Pull date: **2025-09-01** (Africa/Cairo)

---

## License & attribution
- **Charging data**: Open Charge Map (openchargemap.org) — **CC BY 4.0**  
  Please include the credit: **“Contains data © Open Charge Map contributors.”**  
  A copy of the license text is provided in `OCM_CC_BY_4.0.txt`.  
- **EV models** (`ev_models_2025.csv`): compiled from a CC0-friendly public source. No attribution required, though acknowledging the public data commons is appreciated.

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

# Share of high-power DC sites (≥100 kW)
hpc_share = (df["power_kw"] >= 100).mean()
print("HPC share:", round(hpc_share, 3))
