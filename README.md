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

All data are pulled at render-time from the **World Bank Open Data API** (no API key required). API base URL:

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

Cached snapshots of the cleaned data live in `[data/](data/)` (`with_missing_values.csv`, `without_missing_values.csv`, `country_meta.csv`) so the website can be re-rendered offline.

## 📁 Repository layout

```
.
├── index.qmd          # Home page (Quarto source)
├── report.qmd         # Final report — Python execution, renders HTML + PDF
├── viz.qmd            # Three interactive plotly figures
├── _quarto.yml        # Site config (navbar, theme, footer)
├── styles.css         # Editorial design system
├── theme.scss         # Bootstrap variable overrides
├── data/              # Cached World Bank panel (CSV)
├── docs/              # Rendered website (served by GitHub Pages)
```

## 🔁 Reproduce locally

Requires [Quarto](https://quarto.org/) ≥ 1.4 and Python ≥ 3.10 with `pandas`, `numpy`, `requests`, `scikit-learn`, `xgboost`, `plotly`, `plotnine`, `seaborn`, `statsmodels`, `pygam`.

```bash
# Clone
git clone https://github.com/EricWcr7/JSC370-2026S-Final-Project.git
cd JSC370-2026S-Final-Project

# Render the full site (re-fetches API data on the report.qmd run)
quarto render

# Or preview live
quarto preview
```

The rendered output lands in `docs/`; GitHub Pages serves that folder directly.

## 📝 License

See [LICENSE](LICENSE).