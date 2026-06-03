# ⚡ US Power Grid Macro-Dynamics (2010–2026)

A statistically rigorous quantitative analysis of the US national electricity grid, covering generation mix, demand, pricing, emissions, weather forcing, and storage — from January 2010 through **March 2026 (near present-day)**.

---

## Table of Contents

- [Overview](#overview)
- [Data Source](#data-source)
- [Dataset Files](#dataset-files)
- [Project Structure](#project-structure)
- [Analysis Phases](#analysis-phases)
- [Key Findings](#key-findings)
- [Installation](#installation)
- [Usage](#usage)
- [Technical Notes](#technical-notes)

---

## Overview

| | |
|---|---|
| **Author** | Mubin |
| **Date** | May 2026 |
| **Coverage** | January 2010 → March 2026 &nbsp;·&nbsp; **195 monthly observations** |
| **Panel** | `df_master` — 195 months × 31 features, 0 missing values post-imputation |
| **Methodology** | Time-series econometrics · Thermodynamic accounting · Causal inference |
| **Stack** | Python · Pandas · NumPy · SciPy · Statsmodels · Matplotlib · Seaborn |

> **Near real-time coverage:** Data extends through March 2026, sourced directly from the EIA API at collection time. This is not a historical-only study — the panel captures the most recent grid dynamics including post-2021 energy price inflation, accelerating solar buildout, and the ongoing coal phase-out.

---

## Data Source

All data is retrieved directly from the **U.S. Energy Information Administration (EIA) Open Data API v2**.

- **API endpoint:** `https://api.eia.gov/v2/`
- **Authentication:** EIA API key (required — register free at [eia.gov/opendata](https://www.eia.gov/opendata/))
- **Coverage:** 12 separate API series spanning generation, retail sales, pricing, weather, imports/exports, losses, emissions, fuel consumption, and storage
- **Granularity:** Monthly national totals
- **Last pulled:** March 2026

To replicate the data collection, register for a free EIA API key and query each series. The 12 resulting CSVs should be placed in the project directory as described below.

---

## Dataset Files

Place all 12 CSV files in the **same folder as the notebook**, or in a `data/` subfolder. The notebook auto-detects the location — no manual path configuration required.

| File | EIA Series | Description |
|---|---|---|
| `01_grid_generation.csv` | Electricity generation | Generation by fuel type: ALL, FOS, NG, COW, NUC, AOR, HYC, WND, SUN, PET |
| `02_electricity_sales_demand.csv` | Retail sales | Sales by sector: ALL, COM, IND, RES (TWh) |
| `03_electricity_retail_price.csv` | Retail price | Average price by sector (¢/kWh) |
| `04_weather_impact_hdd_cdd.csv` | Weather | Heating & Cooling Degree Days (HDD, CDD) |
| `05_grid_imports_exports.csv` | Trade flows | Grid-level imports and exports (TWh) |
| `06_grid_losses_direct_use.csv` | Losses & direct use | T&D losses (TBtu) and direct use (TWh) |
| `07_*` | ~~Capacity~~ | **Excluded** — EIA extract covers only 2010–2012; 160+ months of spline extrapolation would be physically unreliable |
| `08_carbon_emissions.csv` | CO₂ emissions | National CO₂ emissions (MMT) |
| `09_natural_gas_dynamics.csv` | Natural gas | Production & consumption (Bcf) |
| `10_transportation_ev_energy.csv` | Transportation | EV & transportation energy consumption (TBtu) |
| `11_fuel_consumption.csv` | Fuel consumption | Coal (ktons), natural gas (MMcf) |
| `12_pumped_hydro_storage.csv` | Pumped hydro | Net generation (MWh) — **negative values intentional** (see Technical Notes) |

---

## Project Structure

```
us_power_grid_analysis/
├── us_power_grid_analysis.ipynb   # Main analysis notebook
├── requirements.txt               # Python dependencies
├── README.md                      # This file
├── data/                          # (optional) Place CSVs here
│   ├── 01_grid_generation.csv
│   ├── 02_electricity_sales_demand.csv
│   └── ...
└── plots/                         # Auto-created — all figures saved here
    ├── phase_1_integration_validation.png
    ├── phase_2_thermodynamic_validation.png
    ├── phase_3_energy_transition.png
    ├── phase_4_statistical_inference.png
    └── phase_5_decomposition_volatility.png
```

---

## Analysis Phases

### Phase 1 — Data Integration & Engineering
Loads all 12 CSVs, filters to US national totals, pivots generation wide by `fueltypeid`, and joins into a single `df_master` on a monthly `DatetimeIndex`. Missing values filled with **cubic spline interpolation** (order=3).

### Phase 2 — Thermodynamic Validation
Proves dataset integrity via the EIA electricity flow identity:

```
Supply  = Generation_net + Imports + Pumped_Hydro_net
Demand  = Retail Sales + Exports + Direct Use + T&D Losses
ε_t     = Supply_t − Demand_t  ≈ 5% of Supply
```

Result: ε̄ = 19.24 TWh/mo (5.44%), Pearson r(Supply, Demand) = 0.9791, p < 10⁻¹³⁵

### Phase 3 — Quantitative Trend Analysis
MoM/YoY growth rates, 15-year CAGRs, and market share shifts for 7 generation sources across the full 2010–2026 panel.

### Phase 4 — Statistical Testing & Causal Inference
Four OLS regression models with HC3 heteroskedasticity-robust standard errors. Pearson + Spearman correlation matrices. Breusch-Pagan and Durbin-Watson diagnostics.

### Phase 5 — Time-Series Decomposition & Volatility
STL decomposition (Cleveland et al. 1990, `period=12, robust=True`) on retail sales and price. 12-month rolling σ and coefficient of variation per generation source.

### Phase 6 — Final Quantitative Report
Consolidated summary of all headline statistics re-derived from `df_master`. All figures exported to `plots/` at 300 DPI.

---

## Key Findings

| Finding | Result |
|---|---|
| Coal market share | 44.8% (2010) → 16.6% (2025), CAGR −5.9%/yr |
| Solar CAGR | **+44.3%/yr** — fastest technology penetration in US grid history |
| Renewables share | 4.1% (2010) → 18.6% (2025) |
| Gas crossed coal | **2016** |
| Weather → demand (R²) | 0.860 (bivariate) · 0.983 (full model with FE) |
| +1 CDD marginal effect | +0.283 TWh demand (3.05× stronger than +1 HDD) |
| Fossil CO₂ intensity | 0.809 MMT per TWh fossil (R²=0.709) |
| Demand seasonal strength | F_S = 0.963 — 113.8 TWh amplitude (33% of mean) |
| Solar grid volatility | CoV peaks at 62.2% — 3–4× the total grid CoV of ~9% |
| Energy price break | Flat 2010–2020 (+14%), then +27% in just 3 years post-2021 |

---

## Installation

```bash
git clone https://github.com/your-username/us-power-grid-analysis.git
cd us-power-grid-analysis
pip install -r requirements.txt
```

Then place your 12 EIA CSV files next to the notebook (or in `data/`) and launch:

```bash
jupyter notebook us_power_grid_analysis.ipynb
```

---

## Usage

**Run all phases end-to-end:**
Open the notebook and run all cells in order. Each phase is self-contained with a header, code, and results summary.

**Data directory:** The notebook auto-detects the CSV location by probing for `01_grid_generation.csv` in:
1. The notebook directory
2. A `data/` subfolder
3. `/mnt/user-data/uploads/` (development environment)

**Plot output:** All figures are saved automatically to `plots/` next to the notebook.

---

## Technical Notes

**Pumped hydro negative values**
`12_pumped_hydro_storage.csv` contains negative generation values. These are **not errors** — negative = pumping mode, where the grid consumes electricity to move water uphill for storage. The `clip(lower=0)` line in the imputation step is intentionally commented out to preserve this signal. In the energy balance, the net value is correctly included in Supply (negative net reduces supply during heavy pumping months).

**T&D losses sign convention**
Losses are **added** to the demand side of the energy balance, not subtracted. They represent energy that was generated and transmitted but never reached end users.

**Capacity data (File 07) excluded**
The EIA extract for capacity covers only 2010–2012. Using cubic spline extrapolation to fill the remaining 160+ months would be physically unreliable, so this file is excluded entirely.

**Unit harmonisation**
All generation and sales series are converted to TWh. Prices in ¢/kWh. Emissions in MMT CO₂. Gas in Bcf. Fuel consumption in source units (ktons for coal, MMcf for gas).

**Imputation**
Cubic spline interpolation (`CubicSpline`, order=3) is applied to series with fewer than 4 known points falling back to linear interpolation. Total missing cells pre-imputation: 16. Post-imputation: 0.
