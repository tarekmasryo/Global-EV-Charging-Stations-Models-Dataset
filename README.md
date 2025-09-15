# ⚡ Global EV Charging Stations (2025)  
**Author:** [Tarek Masryo](https://github.com/tarekmasryo) · [Kaggle](https://www.kaggle.com/datasets/tarekmasryo/global-ev-charging-stations)  
**Date:** Sept 2025  

**License:**  
• Charging data (Open Charge Map) — **CC BY 4.0**  
• Companion file `ev_models_2025.csv` — **CC0**  

---

## TL;DR

> Global EV charging snapshot: **242,417 sites** across **121 countries**, with standardized columns for direct use.  
>  
> - Full global dataset (`charging_stations_2025_world.csv`)  
> - ML-ready compact file (`charging_stations_2025_ml.csv`)  
> - Country & world roll-ups (`country_summary_2025.csv`, `world_summary_2025.csv`)  
> - EV models companion file (`ev_models_2025.csv`)  

---

## 🌍 Why this dataset exists
EV charging data is usually fragmented and inconsistent across sources.  
This dataset consolidates everything into a **single, clean, analysis-ready bundle** to explore coverage, ports, and fast-DC availability worldwide.

---

## 📦 What’s inside

### Main file — `charging_stations_2025_world.csv`  
One row per charging site (**11 columns**):  

- `id` (int): OCM unique site ID  
- `name` (str): site name  
- `city` (str) — may be “UNKNOWN”  
- `country_code` (ISO-2)  
- `state_province` (str) — may be “UNKNOWN”  
- `latitude`, `longitude` (float, WGS84)  
- `ports` (int): number of charging points  
- `power_kw` (float): maximum site power (kW)  
- `power_class` (str): derived from `power_kw` thresholds  
- `is_fast_dc` (bool): `true` if `power_kw ≥ 50`  

### ML-ready file — `charging_stations_2025_ml.csv`  
Simplified **7-column** version, deduplicated and compact for direct ML training.  

### Helper files
- `country_summary_2025.csv` — per-country counts & max power  
- `world_summary_2025.csv` — quick global roll-up  

### Companion file
- `ev_models_2025.csv` — EV models (make, model, market regions, powertrain, first year, body style, origin country)  

---

## 🔎 Key Statistics

* **Fast-DC share (≥50 kW):** ~21% of sites  
* **Ports per site:** min **1**, median **11**, mean **35.2**, max **3,160**  
* **Max power (kW):** min **1.2**, median **22**, mean **54.3**, max **2,500**  
* **Top countries by site count:**  
  **US** 82,138 • **GB** 26,825 • **DE** 23,373 • **ES** 17,825 • **CA** 16,490 • **FR** 13,820 • **IT** 10,354 • **NL** 8,091 • **SE** 4,953 • **NO** 4,790  

---

## 🔧 How it was built
- Collected per-country via the **Open Charge Map API** (`/v3/poi`)  
- Deduplicated by unique `id`  
- Dropped rows with invalid coordinates; validated lat/lon ranges  
- Derived fields: `power_class`, `is_fast_dc`  
- Generated helper roll-ups for countries and global coverage  

---

## 🪪 License & attribution
- **Charging data:** Open Charge Map (openchargemap.org) — **CC BY 4.0**  
  Must credit: *“Contains data © Open Charge Map contributors.”*  
- **EV models:** compiled from CC0-friendly sources (no attribution required).  

---

## 💡 Inspiration / Use Cases
- Benchmark EV infrastructure by country or city  
- Map global coverage and fast-DC availability  
- Train clustering or forecasting models using ML-ready file  
- Build dashboards and visualizations (Streamlit, Plotly, Mapbox)  
- Combine with `ev_models_2025.csv` to study **supply vs demand**  

---

## 🚀 Quick start (pandas)
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
