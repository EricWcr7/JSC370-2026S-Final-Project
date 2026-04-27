# JSC370 2026S Final Project — Carbon Dioxide Emissions per Capita, 1990–2023

**Author:** Yi Fan (Eric) Wang
**Course:** JSC370 — Data Science II (Winter 2026), University of Toronto

How have carbon dioxide emissions per capita evolved across countries from 1990 to 2023, and which economic, energy, and land-use factors explain and predict these differences? This project assembles a 34-year country–year panel from the World Bank Open Data API, runs OLS and GAM at the midterm stage, and extends those baselines with a pruned Decision Tree and a tuned XGBoost regressor in the final report.

## 🌐 Website

**Live site:** [https://ericwcr7.github.io/JSC370-2026S-Final-Project/](https://ericwcr7.github.io/JSC370-2026S-Final-Project/)

The site contains:

- **Final Report** — full written analysis ([HTML](https://ericwcr7.github.io/JSC370-2026S-Final-Project/report.html) · [PDF](https://ericwcr7.github.io/JSC370-2026S-Final-Project/report.pdf))
- **Midterm Report** — earlier OLS + GAM analysis ([HTML](https://ericwcr7.github.io/JSC370-2026S-Final-Project/midterm.html))
- **Interactive Visualizations** — three plotly figures ([HTML](https://ericwcr7.github.io/JSC370-2026S-Final-Project/viz.html))

## 📊 Data source — World Bank Open Data API

All data are pulled at render-time from the **World Bank Open Data API** (no API key required). 

API Origin:

[https://data.worldbank.org/](https://data.worldbank.org/)

API base URL:

[https://api.worldbank.org/v2](https://api.worldbank.org/v2)

Indicators used (WDI series codes — each link returns JSON for the country-year panel):


| Variable                           | Indicator code         | API endpoint                                                                                                                                                                                             |
| ---------------------------------- | ---------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CO₂ emissions per capita (outcome) | `EN.GHG.CO2.PC.CE.AR5` | [https://api.worldbank.org/v2/country/all/indicator/EN.GHG.CO2.PC.CE.AR5?format=json&date=1990:2023](https://api.worldbank.org/v2/country/all/indicator/EN.GHG.CO2.PC.CE.AR5?format=json&date=1990:2023) |
| GDP per capita (constant 2015 USD) | `NY.GDP.PCAP.KD`       | [https://api.worldbank.org/v2/country/all/indicator/NY.GDP.PCAP.KD?format=json&date=1990:2023](https://api.worldbank.org/v2/country/all/indicator/NY.GDP.PCAP.KD?format=json&date=1990:2023)             |
| Fossil-fuel electricity share      | `EG.ELC.FOSL.ZS`       | [https://api.worldbank.org/v2/country/all/indicator/EG.ELC.FOSL.ZS?format=json&date=1990:2023](https://api.worldbank.org/v2/country/all/indicator/EG.ELC.FOSL.ZS?format=json&date=1990:2023)             |
| Renewable energy share             | `EG.FEC.RNEW.ZS`       | [https://api.worldbank.org/v2/country/all/indicator/EG.FEC.RNEW.ZS?format=json&date=1990:2023](https://api.worldbank.org/v2/country/all/indicator/EG.FEC.RNEW.ZS?format=json&date=1990:2023)             |
| Forest area (% of land)            | `AG.LND.FRST.ZS`       | [https://api.worldbank.org/v2/country/all/indicator/AG.LND.FRST.ZS?format=json&date=1990:2023](https://api.worldbank.org/v2/country/all/indicator/AG.LND.FRST.ZS?format=json&date=1990:2023)             |
| Urban population share             | `SP.URB.TOTL.IN.ZS`    | [https://api.worldbank.org/v2/country/all/indicator/SP.URB.TOTL.IN.ZS?format=json&date=1990:2023](https://api.worldbank.org/v2/country/all/indicator/SP.URB.TOTL.IN.ZS?format=json&date=1990:2023)       |


Country metadata (used to drop regional aggregates):

[https://api.worldbank.org/v2/country?format=json&per_page=500](https://api.worldbank.org/v2/country?format=json&per_page=500)

API documentation: [https://datahelpdesk.worldbank.org/knowledgebase/topics/125589-developer-information](https://datahelpdesk.worldbank.org/knowledgebase/topics/125589-developer-information)

## 📁 Repository layout

```
.
├── README.md                      # This file
├── LICENSE                        # MIT
├── CLAUDE.md                      # Project context for Claude Code (rubric, skills, midterm recap)
│
├── _quarto.yml                    # Site config (navbar, theme, footer, render list)
├── styles.css                     # Editorial design system (palette, typography, layout)
├── theme.scss                     # Bootstrap variable overrides loaded by Quarto
│
├── index.qmd                      # Home page — hero, story, key findings, site grid
├── report.qmd                     # Final report — Python pipeline, renders HTML + PDF
├── viz.qmd                        # Three interactive plotly figures (map, histogram, scatter)
├── Midterm.qmd                    # Source of the midterm report (rendered HTML lives in docs/)
│
├── data/                          # Cached World Bank panel — re-render works offline
│   ├── with_missing_values.csv    #   full country–year panel, NA preserved
│   ├── without_missing_values.csv #   complete-case sample used for modeling
│   └── country_meta.csv           #   ISO3 → country name + continent (for Interactive Visualizations only)
│
├── docs/                          # Rendered website (GitHub Pages serves this folder)
│   ├── index.html
│   ├── report.html                # Fina report — HTML
│   ├── report.pdf                 # Final report — PDF
│   ├── midterm.html               # Midterm report - HTML
│   ├── viz.html                   # 3 Different Interactive Visualizations
│   ├── styles.css
│   ├── search.json
│   └── site_libs/                 #   Bootstrap, plotly, Quarto runtime assets
```

## 🔁 Reproduce locally

### 1. Prerequisites

- [Quarto](https://quarto.org/) ≥ 1.4
- Python ≥ 3.10 with the packages below
- A LaTeX engine (e.g. [TinyTeX](https://yihui.org/tinytex/) — `quarto install tinytex` is the easiest way) for the PDF render
- An internet connection — `report.qmd` re-fetches indicator data from the World Bank API at render time

### 2. Clone and install Python dependencies

```bash
git clone https://github.com/EricWcr7/JSC370-2026S-Final-Project.git
cd JSC370-2026S-Final-Project

# Create a virtualenv (optional but recommended)
python -m venv .venv
source .venv/bin/activate           # Windows: .venv\Scripts\activate

# Install the packages used across the .qmd files
pip install pandas numpy requests scikit-learn xgboost \
            plotly plotnine seaborn matplotlib \
            statsmodels pygam jupyter
```

### 3. Render the full site (recommended)

```bash
quarto render
```

This rebuilds every page in `_quarto.yml`'s `render:` list — `index.qmd`, `report.qmd`, `viz.qmd` — into `docs/`. The pre-rendered `midterm.html` and `report.pdf` are passed through unchanged. Expect 2–5 minutes the first time (XGBoost tuning + PDF compile).

### 4. Render a single page

You usually only need to rebuild what you changed. Each `.qmd` can be re-rendered on its own:

```bash
# Home page — fast, no Python execution. Re-render after editing
# index.qmd, styles.css, theme.scss, or _quarto.yml.
quarto render index.qmd

# Final report — slow. Runs the full Python pipeline:
#   API fetch  →  cleaning  →  EDA  →  Decision Tree + tuned XGBoost
#   →  evaluation  →  HTML  →  PDF (LaTeX).
# Outputs: docs/report.html and docs/report.pdf.
quarto render report.qmd

# Interactive figures — moderate. Builds the three plotly figures
# (animated world CO2 map, decade histogram, GDP scatter) and
# embeds them into docs/viz.html.
quarto render viz.qmd
```

`midterm.html` is **not** rebuilt by Quarto — it's a pre-rendered artifact carried over from the midterm and copied into `docs/midterm.html` directly.

### 5. Live preview while editing

```bash
quarto preview              # serves the whole site, hot-reloads on save
quarto preview index.qmd    # serve a single page
```

The rendered output always lands in `docs/`; GitHub Pages serves that folder directly.

## 📝 License

See [LICENSE](LICENSE).