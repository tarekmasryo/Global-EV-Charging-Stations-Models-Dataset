# ⚡ Global EV Charging Stations (2025)  
**Author:** [Tarek Masryo](https://github.com/tarekmasryo) · [Kaggle](https://www.kaggle.com/datasets/tarekmasryo/global-ev-charging-stations)  
**Version:** v1.0 (2025-09-01)  

**License:**  
• Charging data (Open Charge Map) — **CC BY 4.0**  
• Companion file `ev_models_2025.csv` — **CC0**  

---

## TL;DR

> Global EV charging snapshot: **242,417 sites** across **121 countries**, with **11 clean columns**.  
>  
> - One global CSV (`charging_stations_2025_world.csv`)  
> - Optional roll-ups (`country_summary_2025.csv`, `world_summary_2025.csv`)  
> - Plus an **EV models** companion file (`ev_models_2025.csv`)  

---

## Why this dataset exists
Electric mobility data is scattered and inconsistent. This dataset provides a **single, analysis-ready CSV** you can load in seconds to explore coverage, power levels, and fast-DC availability worldwide. Snapshot pulled on **2025-09-01**.

---

## What’s inside
**Main file — `charging_stations_2025_world.csv` (one row per site)**  

**Columns (11):**
- `id` (int): OCM unique site ID  
- `name` (str): site name  
- `city` (str)  
- `country_code` (ISO-2)  
- `state_province` (str)  
- `latitude`, `longitude` (float, WGS84)  
- `ports` (int): number of charging points at the site  
- `power_kw` (float): max power (kW) among connectors  
- `power_class` (str): derived from `power_kw` thresholds  
- `is_fast_dc` (bool): `true` if `power_kw ≥ 50`  

**Helper files:**
- `country_summary_2025.csv` — per-country counts & max power  
- `world_summary_2025.csv` — global roll-up  

**Companion file:**
- `ev_models_2025.csv` — EV model records (for context & joinable analysis)

---

## How it was built
- Pulled per-country sites via the Open Charge Map API  
- Deduplicated by `id`, dropped rows without coordinates  
- Derived `power_class` & `is_fast_dc`  
- Generated summaries for country and global coverage  
- Clean-up note: Namibia (`NA`) added, Saint Lucia (`LC`) removed → final = **121 countries**  

---

## License & attribution
- **Charging data**: Open Charge Map (openchargemap.org) — **CC BY 4.0**  
  Must credit: **“Contains data © Open Charge Map contributors.”**  
  A copy of the license text is provided in `OCM_CC_BY_4.0.txt`.  
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

# Share of high-power DC sites (≥100 kW)
hpc_share = (df["power_kw"] >= 100).mean()
print("HPC share:", round(hpc_share, 3))
```
