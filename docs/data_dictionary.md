# Data Dictionary — EV Charging Stations (2025)

## charging_stations_2025_world.csv
| Column         | Type   | Description |
|----------------|--------|-------------|
| id             | int    | Unique station ID (OCM) |
| name           | str    | Station name |
| city           | str    | City name |
| country_code   | str    | ISO-2 country code |
| state_province | str    | State/Province (if available) |
| latitude       | float  | WGS84 latitude |
| longitude      | float  | WGS84 longitude |
| ports          | int    | Number of charging points at the site |
| power_kw       | float  | Maximum charging power (kW) |
| power_class    | str    | Derived class (slow / fast / HPC) |
| is_fast_dc     | bool   | True if `power_kw ≥ 50` |

## country_summary_2025.csv
| Column         | Type   | Description |
|----------------|--------|-------------|
| country        | str    | Country name |
| total_stations | int    | Number of stations |
| avg_ports      | float  | Avg. ports per site |
| avg_power_kw   | float  | Avg. power (kW) |
| ev_adoption_rate | float | Estimated EV adoption rate (%) |

## world_summary_2025.csv
| Column           | Type  | Description |
|------------------|-------|-------------|
| year             | int   | Year of snapshot |
| total_countries  | int   | Countries included |
| total_stations   | int   | Global total stations |
| total_ev_models  | int   | Number of EV models tracked |
| avg_global_power | float | Global average power (kW) |

## ev_models_2025.csv
| Column        | Type  | Description |
|---------------|-------|-------------|
| model_id      | int   | EV model unique ID |
| brand         | str   | Manufacturer |
| model         | str   | Model name |
| battery_kwh   | float | Battery capacity (kWh) |
| max_range_km  | int   | Max driving range (km) |
| fast_charging | bool  | Supports fast charging (yes/no) |
