<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>US Power Grid Macro-Dynamics — README</title>
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    background: #0d1117;
    color: #e6edf3;
    font-family: 'Segoe UI', sans-serif;
    font-size: 14px;
    line-height: 1.6;
    padding: 32px 24px;
    max-width: 960px;
    margin: 0 auto;
  }
  code {
    background: rgba(255,255,255,.1);
    padding: 1px 6px;
    border-radius: 4px;
    font-family: 'Consolas', 'Courier New', monospace;
    font-size: 12px;
    color: #e6edf3;
  }
  pre {
    background: #161b22;
    border-left: 3px solid #4fc3f7;
    border-radius: 8px;
    padding: 16px 20px;
    font-family: 'Consolas', 'Courier New', monospace;
    font-size: 12px;
    color: #8b949e;
    overflow-x: auto;
    margin: 12px 0;
    line-height: 1.7;
  }
  a { color: #58a6ff; }

  /* ── Hero Banner ── */
  .hero {
    background: linear-gradient(135deg,#0f2027,#203a43,#2c5364);
    padding: 48px 40px;
    border-radius: 16px;
    margin-bottom: 20px;
  }
  .hero-tag {
    color: #f5a623;
    font-size: 13px;
    letter-spacing: 4px;
    text-transform: uppercase;
    margin-bottom: 12px;
  }
  .hero h1 {
    color: #fff;
    font-size: 36px;
    font-weight: 800;
    margin-bottom: 10px;
  }
  .hero h2 {
    color: #4fc3f7;
    font-size: 20px;
    font-weight: 400;
    margin-bottom: 28px;
  }
  .hero hr {
    border: none;
    border-top: 1px solid rgba(255,255,255,.15);
    margin-bottom: 24px;
  }
  .meta-table { border-collapse: collapse; width: 100%; }
  .meta-table td { padding: 5px 20px 5px 0; font-size: 14px; }
  .meta-table td:first-child { color: #f5a623; font-weight: 700; white-space: nowrap; }
  .meta-table td:last-child { color: #cfd8dc; }

  /* ── Phase Section Header ── */
  .phase-header {
    background: linear-gradient(135deg,#0f2027,#203a43,#2c5364);
    padding: 32px 40px 28px;
    border-radius: 14px;
    margin: 20px 0 4px;
  }
  .phase-tag {
    font-size: 12px;
    letter-spacing: 4px;
    text-transform: uppercase;
    margin-bottom: 10px;
  }
  .phase-header h2 {
    color: #fff;
    font-size: 22px;
    font-weight: 800;
    margin-bottom: 6px;
  }
  .phase-header p {
    color: #4fc3f7;
    font-size: 14px;
    font-weight: 400;
    margin-bottom: 18px;
  }
  .phase-header hr {
    border: none;
    border-top: 1px solid rgba(255,255,255,.15);
    margin-bottom: 16px;
  }
  .phase-table { border-collapse: collapse; width: 100%; font-size: 13px; }
  .phase-table td { padding: 5px 20px 5px 0; }
  .phase-table td:first-child { color: #f5a623; font-weight: 700; white-space: nowrap; }
  .phase-table td:last-child { color: #cfd8dc; }
  .phase-list { margin: 14px 0 0; padding: 0; list-style: none; font-size: 13px; }
  .phase-list li { padding: 4px 0; color: #b0bec5; }
  .phase-list li span.arrow { color: #4fc3f7; margin-right: 8px; }

  /* ── Info / Whatdoes box ── */
  .info-box {
    background: #161b22;
    padding: 16px 22px;
    border-radius: 8px;
    font-size: 13px;
    color: #8b949e;
    line-height: 1.6;
    margin: 4px 0 12px;
  }
  .info-box.blue  { border-left: 3px solid #4fc3f7; }
  .info-box.green { border-left: 3px solid #3fb950; }
  .info-box.orange{ border-left: 3px solid #ffa657; }
  .info-box.gold  { border-left: 3px solid #f5a623; }
  .info-box b     { color: #e6edf3; }

  /* ── Results Card ── */
  .results-card {
    background: #1a1a14;
    border: 1px solid #3d3520;
    border-top: 3px solid #e6a817;
    border-radius: 10px;
    overflow: hidden;
    margin: 12px 0;
  }
  .results-card-header {
    background: #252010;
    padding: 10px 22px;
    display: flex;
    align-items: center;
    gap: 12px;
  }
  .results-badge {
    background: #e6a817;
    color: #0d0d00;
    font-size: 10px;
    font-weight: 800;
    letter-spacing: 2px;
    padding: 3px 10px;
    border-radius: 20px;
    text-transform: uppercase;
  }
  .results-title { color: #c9a84c; font-size: 13px; font-weight: 600; }
  .results-body { padding: 18px 22px; }
  .results-table { width: 100%; border-collapse: collapse; margin-bottom: 10px; }
  .results-table th {
    padding: 6px 14px 6px 0;
    font-size: 11px;
    color: #8a7a4a;
    border-bottom: 1px solid #3d3520;
    letter-spacing: 1px;
    text-align: left;
  }
  .results-table td {
    padding: 6px 14px 6px 0;
    font-size: 12px;
    color: #b0a06a;
    border-bottom: 1px solid #2a2510;
  }
  .results-table td b   { color: #e6a817; }
  .results-table td.red { color: #c87060; }
  .results-table td.green { color: #7ab870; }
  .results-table td.gold  { color: #e6a817; }
  .bullet-row { display: flex; gap: 10px; padding: 4px 0; }
  .bullet-row .dot { color: #e6a817; margin-top: 1px; flex-shrink: 0; }
  .bullet-row span { color: #b0a06a; font-size: 12px; line-height: 1.6; }
  .bullet-row span b { color: #d4c07a; }

  /* ── Color Palette Grid ── */
  .palette-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(160px, 1fr));
    gap: 10px;
    margin: 14px 0;
  }
  .swatch {
    border-radius: 8px;
    overflow: hidden;
    border: 1px solid #30363d;
  }
  .swatch-color { height: 48px; }
  .swatch-label {
    background: #161b22;
    padding: 6px 10px;
    font-size: 11px;
    color: #8b949e;
  }
  .swatch-label b { color: #e6edf3; display: block; font-size: 12px; }

  /* ── Roadmap grid ── */
  .roadmap-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 8px;
    font-size: 13px;
    color: #b0bec5;
    margin-top: 14px;
  }

  /* ── Warning box ── */
  .warning-box {
    background: rgba(255,166,87,.07);
    border-left: 3px solid #f5a623;
    padding: 14px 20px;
    border-radius: 6px;
    color: #e0f7fa;
    font-size: 13px;
    font-family: monospace;
    margin: 10px 0;
  }

  /* ── Section divider ── */
  .section-divider {
    border: none;
    border-top: 1px solid #21262d;
    margin: 28px 0;
  }
</style>
</head>
<body>

<!-- ── HERO ── -->
<div class="hero">
  <p class="hero-tag">Scientific Data Analysis &middot; Energy Analytics</p>
  <h1>&#9889; US Power Grid Macro-Dynamics</h1>
  <h2>A Statistically Rigorous Quantitative Analysis &nbsp;(2010&ndash;2026)</h2>
  <hr>
  <table class="meta-table">
    <tr><td>Author</td><td>Mubin</td></tr>
    <tr><td>Date</td><td>May 2026</td></tr>
    <tr><td>Scope</td><td>National-level monthly panel &mdash; 195 observations (2010-01 &rarr; 2026-03)</td></tr>
    <tr><td>Sources</td><td>12 EIA datasets: Generation, Demand, Price, Weather, Emissions, Infrastructure</td></tr>
    <tr><td>Methodology</td><td>Time-series econometrics &middot; Thermodynamic accounting &middot; Causal inference</td></tr>
    <tr><td>Stack</td><td>Python &middot; Pandas &middot; NumPy &middot; SciPy &middot; Statsmodels &middot; Matplotlib &middot; Seaborn</td></tr>
    <tr><td>Master Frame</td><td><code>df_master</code> &mdash; 195 months &times; 31 features, 0 missing values post-imputation</td></tr>
  </table>
  <hr style="margin-top:24px;">
  <h3 style="color:#80cbc4;font-size:15px;font-weight:600;margin-bottom:14px;">&#9654; Analysis Roadmap</h3>
  <div class="roadmap-grid">
    <div>&#9312; Data Integration &amp; Engineering</div>
    <div>&#9316; Statistical Testing &amp; Causal Inference</div>
    <div>&#9313; Thermodynamic Validation &amp; Error Margins</div>
    <div>&#9317; Time-Series Decomposition &amp; Volatility</div>
    <div>&#9314; Quantitative Trend Analysis (The Transition)</div>
    <div>&#9318; Final Scientific Report &amp; Export</div>
  </div>
</div>

<div class="info-box blue">
  CSVs are auto-located from the notebook directory (or a <code>data/</code> subfolder). A <code>plots/</code> folder is created next to the notebook — all figures save there.
</div>

<hr class="section-divider">

<!-- ── DATASET FILES ── -->
<div class="phase-header">
  <p class="phase-tag" style="color:#f5a623;">US Power Grid &middot; 2010&ndash;2026</p>
  <h2>&#128193; Dataset Files</h2>
  <p>12 Required EIA CSV Files</p>
  <hr>
  <table class="phase-table">
    <tr><td>File 01</td><td><code>01_grid_generation.csv</code> &mdash; Generation by fuel type (ALL, FOS, NG, COW, NUC, AOR, HYC, WND, SUN, PET)</td></tr>
    <tr><td>File 02</td><td><code>02_electricity_sales_demand.csv</code> &mdash; Retail sales by sector (ALL, COM, IND, RES) in TWh</td></tr>
    <tr><td>File 03</td><td><code>03_electricity_retail_price.csv</code> &mdash; Average retail price by sector in &cent;/kWh</td></tr>
    <tr><td>File 04</td><td><code>04_weather_impact_hdd_cdd.csv</code> &mdash; Heating &amp; Cooling Degree Days (HDD, CDD)</td></tr>
    <tr><td>File 05</td><td><code>05_grid_imports_exports.csv</code> &mdash; Grid-level imports and exports in TWh</td></tr>
    <tr><td>File 06</td><td><code>06_grid_losses_direct_use.csv</code> &mdash; T&amp;D losses (TBtu) and direct use (TWh)</td></tr>
    <tr><td>File 07</td><td style="color:#c87060;"><code>07_*</code> &mdash; <b style="color:#c87060;">Excluded</b> — EIA extract covers only 2010&ndash;2012; 160+ months of spline extrapolation physically unreliable</td></tr>
    <tr><td>File 08</td><td><code>08_carbon_emissions.csv</code> &mdash; CO&#8322; emissions in MMT</td></tr>
    <tr><td>File 09</td><td><code>09_natural_gas_dynamics.csv</code> &mdash; Gas production &amp; consumption in Bcf</td></tr>
    <tr><td>File 10</td><td><code>10_transportation_ev_energy.csv</code> &mdash; Transportation/EV energy in TBtu</td></tr>
    <tr><td>File 11</td><td><code>11_fuel_consumption.csv</code> &mdash; Fuel consumption by type (coal ktons, gas MMcf)</td></tr>
    <tr><td>File 12</td><td><code>12_pumped_hydro_storage.csv</code> &mdash; Pumped hydro net generation (MWh). <b style="color:#e6a817;">Negative values are intentional</b> &mdash; pumping mode.</td></tr>
  </table>
  <ul class="phase-list" style="margin-top:16px;">
    <li><span class="arrow">&#9654;</span>Place all files in the <b style="color:#fff;">same folder as the notebook</b>, or in a <code>data/</code> subfolder</li>
    <li><span class="arrow">&#9654;</span>The notebook auto-detects the path via <code>_find_data_dir()</code> — no manual path config needed</li>
  </ul>
</div>

<hr class="section-divider">

<!-- ── PHASE 1 ── -->
<div class="phase-header">
  <p class="phase-tag" style="color:#f5a623;">US Power Grid &middot; 2010&ndash;2026</p>
  <h2>&#9776; Phase 1 &mdash; Data Integration &amp; Engineering</h2>
  <p>Loading &middot; Merging &middot; Imputing &middot; Harmonising 12 EIA Datasets</p>
  <hr>
  <table class="phase-table">
    <tr><td>Objective</td><td>Construct a single, temporally-aligned <code>df_master</code> from 12 heterogeneous EIA datasets</td></tr>
    <tr><td>Date Range</td><td>2010-01 &rarr; 2026-03 &nbsp;(195 monthly observations)</td></tr>
    <tr><td>Unit Harmonisation</td><td>Generation/Sales &rarr; TWh &nbsp;|&nbsp; Prices &rarr; &cent;/kWh &nbsp;|&nbsp; Emissions &rarr; MMT CO&#8322;</td></tr>
    <tr><td>Imputation</td><td>Cubic spline interpolation (order=3) — <code>clip(lower=0)</code> commented out to preserve pumped hydro negatives</td></tr>
  </table>
</div>
<div class="info-box orange"><b>What this does:</b> Loads all 12 CSVs, filters to US national totals, pivots generation wide by <code>fueltypeid</code>, joins everything into <code>df_master</code> on a monthly DatetimeIndex, then runs cubic spline imputation on any gaps.</div>

<div class="results-card">
  <div class="results-card-header">
    <span class="results-badge">Results</span>
    <span class="results-title">Phase 1 &mdash; Integration Quality Assessment</span>
  </div>
  <div class="results-body">
    <table class="results-table">
      <tr><td>Panel dimensions</td><td>195 months &times; 31 features</td></tr>
      <tr><td>Date range</td><td>2010-01 &rarr; 2026-03</td></tr>
      <tr><td>Missing pre-impute</td><td>16 cells</td></tr>
      <tr><td>Missing post-impute</td><td><b>0</b> &mdash; cubic spline interpolation</td></tr>
    </table>
    <div class="bullet-row"><span class="dot">&#9658;</span><span>Generation mean <b>345 TWh/mo</b>, &sigma; = 36.6 TWh &mdash; seasonal amplitude &plusmn;10.6%</span></div>
    <div class="bullet-row"><span class="dot">&#9658;</span><span>Renewable generation: ~11 TWh/mo (2010) &rarr; ~86 TWh/mo (2026) &mdash; <b>8&times; absolute increase</b></span></div>
    <div class="bullet-row"><span class="dot">&#9658;</span><span>Solar trajectory near-exponential post-2018; CO&#8322; shows clear secular decline alongside</span></div>
  </div>
</div>

<hr class="section-divider">

<!-- ── PHASE 2 ── -->
<div class="phase-header">
  <p class="phase-tag" style="color:#4fc3f7;">US Power Grid &middot; 2010&ndash;2026</p>
  <h2>&#9878; Phase 2 &mdash; Thermodynamic Validation &amp; Error Margins</h2>
  <p>Proving Dataset Integrity via the EIA Electricity Flow Identity</p>
  <hr>
  <div class="warning-box">Supply &nbsp;= Generation<sub>net</sub> + Imports + Pumped_Hydro_net</div>
  <div class="warning-box">Demand = Retail Sales + Exports + Direct Use + T&amp;D Losses</div>
  <div class="warning-box">&epsilon;<sub>t</sub> = Supply<sub>t</sub> &minus; Demand<sub>t</sub> &nbsp;&asymp;&nbsp; 5% of Supply (T&amp;D losses)</div>
  <p style="color:#90a4ae;font-size:12px;margin-top:12px;font-style:italic;">
    &nbsp;&#9888;&nbsp; Losses sign convention: T&amp;D losses are <b style="color:#cfd8dc;">added</b> to demand, not subtracted &mdash; they represent generated energy that was never delivered.
  </p>
  <p style="color:#90a4ae;font-size:12px;margin-top:6px;font-style:italic;">
    &nbsp;&#9888;&nbsp; Pumped hydro net is included in Supply: positive when generating, negative when consuming (pumping mode). Do <b style="color:#cfd8dc;">not</b> clip to zero.
  </p>
</div>

<div class="results-card">
  <div class="results-card-header">
    <span class="results-badge">Results</span>
    <span class="results-title">Phase 2 &mdash; Thermodynamic Validation</span>
  </div>
  <div class="results-body">
    <table class="results-table">
      <tr><td>Supply &mu;</td><td>~350 TWh/mo &mdash; generation + imports</td></tr>
      <tr><td>Demand &mu;</td><td>~330 TWh/mo &mdash; sales + exports + direct use</td></tr>
      <tr><td>Residual &epsilon;&#772;</td><td><b>19.24 TWh (5.44%)</b> &mdash; implicit T&amp;D losses</td></tr>
      <tr><td>Residual &sigma;</td><td>7.97 TWh (2.0%) &mdash; seasonal variation in line losses</td></tr>
      <tr><td>Pearson r(S, D)</td><td><b>0.9791</b>, p &lt; 10&sup2;&#8315;&#185;&#179;&#8309;</td></tr>
      <tr><td>t-test H&#8320;: &mu;%=5%</td><td>t=3.01, p=0.003 &mdash; reject H&#8320;; mean slightly above EIA benchmark</td></tr>
    </table>
    <div class="bullet-row"><span class="dot">&#9658;</span><span>Balance closes within EIA's 4&ndash;6% T&amp;D band across all 193 months &mdash; <b>dataset integrity confirmed</b></span></div>
    <div class="bullet-row"><span class="dot">&#9658;</span><span>Residual uptick post-2020 consistent with distributed rooftop solar estimation lag</span></div>
  </div>
</div>

<hr class="section-divider">

<!-- ── PHASE 3 ── -->
<div class="phase-header">
  <p class="phase-tag" style="color:#f5a623;">US Power Grid &middot; 2010&ndash;2026</p>
  <h2>&#9196; Phase 3 &mdash; Quantitative Trend Analysis</h2>
  <p>The Energy Transition: Fossil Collapse &amp; Renewable Hypergrowth</p>
  <hr>
  <table class="phase-table">
    <tr><td>MoM Growth</td><td>g<sup>MoM</sup><sub>t</sub> = (x<sub>t</sub> &minus; x<sub>t&minus;1</sub>) / x<sub>t&minus;1</sub> &times; 100</td></tr>
    <tr><td>YoY Growth</td><td>g<sup>YoY</sup><sub>t</sub> = (x<sub>t</sub> &minus; x<sub>t&minus;12</sub>) / x<sub>t&minus;12</sub> &times; 100</td></tr>
    <tr><td>CAGR</td><td>(x<sub>2025</sub> / x<sub>2010</sub>)<sup>1/15</sup> &minus; 1</td></tr>
    <tr><td>Market Share</td><td>s<sup>(k)</sup><sub>t</sub> = gen<sup>(k)</sup><sub>t</sub> / gen<sup>total</sup><sub>t</sub> &times; 100</td></tr>
  </table>
</div>

<div class="results-card">
  <div class="results-card-header">
    <span class="results-badge">Results</span>
    <span class="results-title">Phase 3 &mdash; Energy Transition Scorecard (2010 &rarr; 2025)</span>
  </div>
  <div class="results-body">
    <table class="results-table">
      <thead><tr><th>Source</th><th>2010 Share</th><th>2025 Share</th><th>&Delta; (pp)</th><th>CAGR</th></tr></thead>
      <tr><td class="red">Coal</td><td class="red">44.8%</td><td class="red">16.6%</td><td class="red"><b>&minus;28.2 pp</b></td><td class="red">&minus;5.9% / yr</td></tr>
      <tr><td>Natural Gas</td><td>23.9%</td><td>40.8%</td><td>+16.9 pp</td><td>+3.9% / yr</td></tr>
      <tr><td>Nuclear</td><td>19.6%</td><td>17.7%</td><td>&minus;1.9 pp</td><td>&minus;0.2% / yr</td></tr>
      <tr><td class="green">Renewables (all)</td><td class="green">4.1%</td><td class="green">18.6%</td><td class="green"><b>+14.5 pp</b></td><td class="green"><b>+11.2% / yr</b></td></tr>
      <tr><td class="green">&nbsp;&nbsp;Wind</td><td class="green">2.3%</td><td class="green">10.5%</td><td class="green">+8.2 pp</td><td class="green">+11.2% / yr</td></tr>
      <tr><td class="gold">&nbsp;&nbsp;Solar</td><td class="gold">~0%</td><td class="gold">6.7%</td><td class="gold">+6.7 pp</td><td class="gold"><b>+44.3% / yr</b></td></tr>
    </table>
    <div class="bullet-row"><span class="dot">&#9658;</span><span>Gas crossed coal in share in <b>2016</b> &mdash; the shale bridge</span></div>
    <div class="bullet-row"><span class="dot">&#9658;</span><span>Solar's <b>44.3% CAGR</b> is the fastest technology penetration in US grid history</span></div>
  </div>
</div>

<hr class="section-divider">

<!-- ── PHASE 4 ── -->
<div class="phase-header">
  <p class="phase-tag" style="color:#ce93d8;">US Power Grid &middot; 2010&ndash;2026</p>
  <h2>&#128202; Phase 4 &mdash; Statistical Testing &amp; Causal Inference</h2>
  <p>Weather &rarr; Demand &rarr; Price: OLS Regression &amp; Hypothesis Testing</p>
  <hr>
  <table class="phase-table">
    <tr><td>Step 1</td><td>Pearson + Spearman correlation matrix across core variable set</td></tr>
    <tr><td>Step 2</td><td>Bivariate OLS &mdash; marginal effect of +1 CDD/HDD on demand and price</td></tr>
    <tr><td>Step 3</td><td>Multivariate OLS with month fixed effects + trend &mdash; HC3 heteroskedasticity-robust SE</td></tr>
    <tr><td>Step 4</td><td>Diagnostics: Breusch-Pagan, Durbin-Watson, condition number</td></tr>
    <tr><td>Step 5</td><td>Auxiliary: CO&#8322; ~ Fossil generation (emissions intensity validation)</td></tr>
  </table>
  <p style="color:#90a4ae;font-size:12px;margin-top:14px;font-style:italic;">
    &#9888; Endogeneity note: Price and demand are jointly determined; OLS price coefficient = conditional correlation, not structural elasticity.
  </p>
</div>

<div class="results-card">
  <div class="results-card-header">
    <span class="results-badge">Results</span>
    <span class="results-title">Phase 4 &mdash; Statistical Testing &amp; Causal Inference</span>
  </div>
  <div class="results-body">
    <table class="results-table">
      <thead><tr><th>Model</th><th>R&sup2;</th><th>Key Finding</th></tr></thead>
      <tr><td>M1: Sales ~ CDD + HDD</td><td><b>0.860</b></td><td>Weather alone explains 86% of monthly demand variance</td></tr>
      <tr><td>M2: Full model + FE + trend</td><td><b>0.983</b></td><td>Adj R&sup2; = 0.982 &mdash; near-complete explanation</td></tr>
      <tr><td>M3: Price ~ Weather + FE</td><td><b>0.796</b></td><td>CDD insignificant once seasonality removed</td></tr>
      <tr><td>M4: CO&#8322; ~ Fossil generation</td><td><b>0.709</b></td><td>0.809 MMT CO&#8322; per TWh fossil</td></tr>
    </table>
    <div class="bullet-row"><span class="dot">&#9658;</span><span>+1 CDD &rarr; <b>+0.283 TWh</b> demand (t=14.3, p&lt;10&sup2;&#8315;&#8308;&#8310;, HC3 robust)</span></div>
    <div class="bullet-row"><span class="dot">&#9658;</span><span>+1 HDD &rarr; <b>+0.093 TWh</b> demand &mdash; cooling effect is <b>3.05&times;</b> heating</span></div>
    <div class="bullet-row"><span class="dot">&#9658;</span><span>Breusch-Pagan p&gt;0.05 on all models &mdash; homoskedastic; HC3 SEs are conservative</span></div>
  </div>
</div>

<hr class="section-divider">

<!-- ── PHASE 5 ── -->
<div class="phase-header">
  <p class="phase-tag" style="color:#80cbc4;">US Power Grid &middot; 2010&ndash;2026</p>
  <h2>&#128200; Phase 5 &mdash; Time-Series Decomposition &amp; Volatility</h2>
  <p>STL Decomposition &middot; Rolling &sigma; &middot; Grid Stress Periods</p>
  <hr>
  <table class="phase-table">
    <tr><td>STL Decomposition</td><td>Seasonal-Trend via LOESS (Cleveland et al. 1990) &mdash; robust to outliers, handles non-stationary seasonality</td></tr>
    <tr><td>Seasonal Strength</td><td>F<sub>S</sub> = max(0, 1 &minus; Var(R<sub>t</sub>) / Var(S<sub>t</sub> + R<sub>t</sub>))</td></tr>
    <tr><td>Rolling Volatility</td><td>12-month rolling &sigma; &amp; CoV = &sigma;/&mu; &times; 100 per generation source</td></tr>
    <tr><td>Parameters</td><td><code>STL(period=12, robust=True)</code> applied to retail sales and price series</td></tr>
  </table>
</div>

<div class="results-card">
  <div class="results-card-header">
    <span class="results-badge">Results</span>
    <span class="results-title">Phase 5 &mdash; STL Decomposition &amp; Volatility</span>
  </div>
  <div class="results-body">
    <table class="results-table">
      <tr><td>Sales &mdash; trend range</td><td>309 &ndash; 341 TWh/mo</td></tr>
      <tr><td>Sales &mdash; seasonal amplitude</td><td><b>113.8 TWh</b> (33% of mean)</td></tr>
      <tr><td>Sales &mdash; seasonal strength</td><td><b>F<sub>S</sub> = 0.963</b> &mdash; near-total seasonal dominance</td></tr>
      <tr><td>Sales &mdash; remainder &sigma;</td><td>6.15 TWh</td></tr>
      <tr><td>Price &mdash; trend range</td><td>9.77 &rarr; 14.22 &cent;/kWh (+45% over 15 years)</td></tr>
      <tr><td>Price &mdash; seasonal strength</td><td>F<sub>S</sub> = 0.775</td></tr>
      <tr><td>Solar 12-mo rolling CoV</td><td><b>peaks at 62.2%</b> &rarr; stabilises ~25&ndash;30%</td></tr>
    </table>
    <div class="bullet-row"><span class="dot">&#9658;</span><span>Demand seasonality is the single strongest signal in the dataset &mdash; driven entirely by AC load</span></div>
    <div class="bullet-row"><span class="dot">&#9658;</span><span>Price trend near-flat 2010&ndash;2020 (+14%), then sharp break: <b>+27% in just 3 years</b> post-2021</span></div>
    <div class="bullet-row"><span class="dot">&#9658;</span><span>Solar CoV 3&ndash;4&times; total grid CoV (~9%) &mdash; quantitative case for battery storage buildout</span></div>
    <div class="bullet-row"><span class="dot">&#9658;</span><span>COVID-19 April 2020: &minus;18 TWh remainder spike &asymp; 3&sigma; &mdash; largest structural shock in the panel</span></div>
  </div>
</div>

<hr class="section-divider">

<!-- ── PHASE 6 ── -->
<div class="phase-header">
  <p class="phase-tag" style="color:#ef9a9a;">US Power Grid &middot; 2010&ndash;2026</p>
  <h2>&#128200; Phase 6 &mdash; Final Quantitative Report &amp; Export <span style="background:#ef9a9a;color:#0f2027;font-size:11px;font-weight:700;padding:3px 10px;border-radius:20px;letter-spacing:1px;text-transform:uppercase;margin-left:10px;vertical-align:middle;">Final</span></h2>
  <p>Consolidated Findings &middot; Statistical Summary &middot; Dataset Export</p>
  <hr>
  <table class="phase-table">
    <tr><td>Output</td><td>All headline statistics re-derived from <code>df_master</code> &mdash; CAGR benchmarks, CO&#8322; savings, OLS coefficients, STL metrics</td></tr>
    <tr><td>Plots dir</td><td><code>plots/</code> &mdash; 5 figures at 300 DPI saved next to the notebook</td></tr>
  </table>
  <ul class="phase-list" style="margin-top:14px;">
    <li><span class="arrow">&#9654;</span><code>phase_1_integration_validation.png</code> &mdash; 6-panel core series overview</li>
    <li><span class="arrow">&#9654;</span><code>phase_2_thermodynamic_validation.png</code> &mdash; Energy balance, residuals, OLS scatter</li>
    <li><span class="arrow">&#9654;</span><code>phase_3_energy_transition.png</code> &mdash; Stacked area, market share, CAGR bars</li>
    <li><span class="arrow">&#9654;</span><code>phase_4_statistical_inference.png</code> &mdash; Correlation heatmaps, OLS scatter, residuals</li>
    <li><span class="arrow">&#9654;</span><code>phase_5_decomposition_volatility.png</code> &mdash; STL components, rolling &sigma; and CoV</li>
  </ul>
</div>

<hr class="section-divider">

<!-- ── VISUAL THEME ── -->
<div class="phase-header">
  <p class="phase-tag" style="color:#4fc3f7;">Notebook Design</p>
  <h2>&#127912; Visual Theme &amp; Color Palette</h2>
  <p>Dark Publication Theme &mdash; consistent across all phases</p>
  <hr>
  <table class="phase-table">
    <tr><td>Figure BG</td><td><code>#0d1117</code> &mdash; DARK_BG</td></tr>
    <tr><td>Axes BG</td><td><code>#161b22</code> &mdash; PANEL_BG</td></tr>
    <tr><td>Grid lines</td><td><code>#21262d</code> &mdash; dashed, alpha 0.6</td></tr>
    <tr><td>All text</td><td><code>#e6edf3</code> &mdash; TEXT_COL</td></tr>
    <tr><td>Axis spines</td><td><code>#30363d</code> &mdash; SPINE_COL (top + right spines hidden)</td></tr>
    <tr><td>Font</td><td>DejaVu Sans &middot; title 13pt &middot; labels 11pt &middot; DPI 130 (screen) / 300 (saved)</td></tr>
  </table>
</div>

<div class="info-box blue" style="margin-top:4px;">
  <b>Accent Palette</b> &mdash; applied consistently: blue for generation/gas, green for renewables, red-orange for fossil/negative residuals, orange for solar/warnings, purple for HDD scatter.
</div>

<div class="palette-grid" style="margin-top:12px;">
  <div class="swatch"><div class="swatch-color" style="background:#58a6ff;"></div><div class="swatch-label"><b>ACCENT1</b>#58a6ff &mdash; Blue<br>Total gen, gas</div></div>
  <div class="swatch"><div class="swatch-color" style="background:#3fb950;"></div><div class="swatch-label"><b>ACCENT2</b>#3fb950 &mdash; Green<br>Renewables, hydro</div></div>
  <div class="swatch"><div class="swatch-color" style="background:#f78166;"></div><div class="swatch-label"><b>ACCENT3</b>#f78166 &mdash; Red/Orange<br>Fossil, neg residuals</div></div>
  <div class="swatch"><div class="swatch-color" style="background:#d2a8ff;"></div><div class="swatch-label"><b>ACCENT4</b>#d2a8ff &mdash; Purple<br>HDD scatter</div></div>
  <div class="swatch"><div class="swatch-color" style="background:#ffa657;"></div><div class="swatch-label"><b>ACCENT5</b>#ffa657 &mdash; Orange<br>Solar, warnings</div></div>
  <div class="swatch"><div class="swatch-color" style="background:#79c0ff;"></div><div class="swatch-label"><b>ACCENT6</b>#79c0ff &mdash; Light blue<br>Secondary series</div></div>
  <div class="swatch"><div class="swatch-color" style="background:#6e7681;"></div><div class="swatch-label"><b>C_COAL</b>#6e7681 &mdash; Grey<br>Coal</div></div>
  <div class="swatch"><div class="swatch-color" style="background:#a5d6ff;"></div><div class="swatch-label"><b>C_NUC</b>#a5d6ff &mdash; Pale blue<br>Nuclear</div></div>
  <div class="swatch"><div class="swatch-color" style="background:#56d364;"></div><div class="swatch-label"><b>C_WIND</b>#56d364 &mdash; Bright green<br>Wind</div></div>
  <div class="swatch"><div class="swatch-color" style="background:#e6a817;"></div><div class="swatch-label"><b>Gold</b>#e6a817 &mdash; Gold<br>Results badges</div></div>
  <div class="swatch"><div class="swatch-color" style="background:#4fc3f7;"></div><div class="swatch-label"><b>Cyan</b>#4fc3f7 &mdash; Cyan<br>Section subtitles</div></div>
  <div class="swatch"><div class="swatch-color" style="background:#80cbc4;"></div><div class="swatch-label"><b>Teal</b>#80cbc4 &mdash; Teal<br>Phase 5 tag</div></div>
</div>

<hr class="section-divider">

<!-- ── SETUP ── -->
<div class="phase-header">
  <p class="phase-tag" style="color:#3fb950;">Setup &amp; Installation</p>
  <h2>&#9881; Getting Started</h2>
  <p>Install dependencies and run</p>
  <hr>
  <table class="phase-table">
    <tr><td>Python</td><td>&ge; 3.9 recommended</td></tr>
    <tr><td>Install</td><td><code>pip install -r requirements.txt</code></td></tr>
    <tr><td>Run</td><td><code>jupyter notebook us_power_grid_analysis.ipynb</code></td></tr>
    <tr><td>Data</td><td>Place all 12 CSVs next to the notebook or in <code>data/</code></td></tr>
  </table>
</div>

<div class="info-box green" style="margin-top:4px;">
  <b>Important Notes:</b><br>
  &nbsp;&#9658;&nbsp; <b>Pumped hydro negative values are intentional</b> &mdash; do not clip. Negative = pumping mode (grid storing energy).<br>
  &nbsp;&#9658;&nbsp; <b>Losses sign:</b> T&amp;D losses are <i>added</i> to demand in the energy balance equation, not subtracted.<br>
  &nbsp;&#9658;&nbsp; <b>File 07 excluded:</b> capacity data covers only 2010&ndash;2012 in the EIA extract.
</div>

</body>
</html>
