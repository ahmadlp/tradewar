# The Cost of a Global Tariff War — Interactive Dashboard

An interactive data visualization dashboard for the paper:

> **"The Cost of a Global Tariff War: A Sufficient Statistics Approach"**
> Ahmad Lashkaripour, *Journal of International Economics*, 2021
> [https://doi.org/10.1016/j.jinteco.2020.103419](https://doi.org/10.1016/j.jinteco.2020.103419)

The dashboard presents the paper's quantitative findings on Nash tariff equilibria across 44 countries, 16 sectors, and the 2000–2014 period, allowing readers to explore the results interactively.

## Live Demo

[View the dashboard &rarr;](#) <!-- Replace with your hosted URL -->

## Features

- **Global Cost (Figure 2)** — Line chart showing the total welfare cost of a tariff war under four model specifications (Baseline, Baseline + Distortions, Baseline + Input Trade, Integrated), with a fixed Y-axis for cross-specification comparison.
- **World Map** — Choropleth showing country-level GDP impact. Click any country to explore its time series.
- **Country Table** — Sortable table of Nash tariffs and GDP changes across model specifications.
- **Country Deep Dive** — Select up to 5 countries to compare their welfare trajectories over time. Single-country view shows all four models; multi-country view uses the selected model.
- **Sectoral Trade Elasticities** — Horizontal bar chart of the 16 estimated sector-level trade elasticities.
- **Cooperation Gains (Figure 5)** — Area chart showing global welfare gains from cooperative (zero) tariffs vs. Nash equilibrium, rising from $184B (2000) to $347B (2014).
- **Winners and Losers** — Ranked bar chart of country-level welfare impacts.
- **Trade Elasticity Toggle** — Switch between the paper's estimated sector-specific elasticities and a uniform elasticity (&#949; = 3 to 5 in 0.5 increments) computed from a pre-solved Nash equilibrium grid. The toggle sits in the sticky navigation bar and reactively updates all exhibits.

## Tech Stack

| Layer | Tool |
|-------|------|
| Framework | [Vue 3](https://vuejs.org/) (Composition API) |
| Build | [Vite](https://vite.dev/) |
| State | [Pinia](https://pinia.vuejs.org/) |
| Charts | [D3.js v7](https://d3js.org/) (direct SVG) |
| Map | [TopoJSON / world-atlas](https://github.com/topojson/world-atlas) |
| CSS | [UnoCSS](https://unocss.dev/) with custom NYT-inspired design tokens |
| Fonts | Source Serif 4, Inter (Google Fonts) |

## Project Structure

```
dashboard/
├── index.html              # Entry HTML
├── vite.config.js          # Vite config (base: './' for relative paths)
├── uno.config.js           # UnoCSS design tokens and shortcuts
├── package.json
├── src/
│   ├── main.js             # App entry point
│   ├── App.vue             # Root component
│   ├── stores/
│   │   └── dashboard.js    # Pinia store (shared state, data loading)
│   ├── utils/
│   │   └── d3helpers.js    # Reusable D3 utilities (axes, tooltips, responsive SVG)
│   ├── components/
│   │   ├── HeroHeader.vue          # Title, author, summary statistics
│   │   ├── StickyNav.vue           # Section links + trade elasticity controls
│   │   ├── GlobalCost.vue          # Figure 2: multi-model line chart
│   │   ├── WorldMap.vue            # Choropleth map
│   │   ├── CountryTable.vue        # Sortable data table
│   │   ├── CountryDeepDive.vue     # Multi-country time series comparison
│   │   ├── SectorElasticities.vue  # Horizontal bar chart of sector elasticities
│   │   ├── CooperationGains.vue    # Figure 5: cooperation gains area chart
│   │   └── WinnersLosers.vue       # Ranked diverging bar chart
│   └── data/
│       ├── estimated.json   # 2,640 records — paper's baseline results
│       ├── uniform.json     # 13,200 records — pre-computed uniform-elasticity grid
│       ├── table1.json      # Nash tariffs and GDP changes (Table 1)
│       ├── countries.json   # 44 countries (ISO3 codes + names)
│       ├── sectors.json     # 16 sectors with estimated elasticities
│       └── cooperation.json # Annual cooperation gains ($B)
└── dist/                    # Production build output
```

## Data

All data is embedded in JSON files at build time. No external API calls or runtime data loading is required (except the world map geometry from `cdn.jsdelivr.net`).

| File | Records | Description |
|------|---------|-------------|
| `estimated.json` | 2,640 | Paper's results: 44 countries &times; 15 years &times; 4 models |
| `uniform.json` | 13,200 | Nash equilibria under uniform elasticity: 44 &times; 15 &times; 4 &times; 5 elasticity values |
| `table1.json` | 43 | Cross-sectional tariff and GDP data (Table 1) |
| `countries.json` | 44 | Country metadata |
| `sectors.json` | 16 | Sector names and estimated elasticities |
| `cooperation.json` | 15 | Annual cooperation gains |

The uniform elasticity grid was pre-computed in MATLAB by solving for Nash equilibrium tariffs at each point in {3, 3.5, 4, 4.5, 5} &times; 4 models &times; 15 years &times; 44 countries using `fsolve`.

## Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) 18+

### Development

```bash
cd dashboard
npm install
npm run dev
```

Open [http://localhost:5173](http://localhost:5173) in your browser.

### Production Build

```bash
npm run build
```

The output is written to `dist/`. All asset paths are relative (`base: './'`), so the build can be deployed to any subdirectory.

### Deployment

Copy the contents of `dist/` to any static hosting provider:

```bash
# Example: deploy to a subdirectory on your server
scp -r dist/* user@server:/var/www/html/tariff-dashboard/
```

The dashboard is a fully static site — no server-side runtime is needed.

> **Note:** Opening `dist/index.html` directly via the `file://` protocol will show a blank page because browsers block ES module imports from local files. Always serve through an HTTP server.

## Model Specifications

The paper analyzes four nested model specifications:

1. **Baseline** — Standard Ricardian trade model
2. **Baseline + Distortions** — Adds sector-level markup distortions
3. **Baseline + Input Trade** — Adds inter-sector input-output linkages
4. **Integrated** — Combines both distortions and input-output trade (most comprehensive)

## Citation

```bibtex
@article{lashkaripour2021cost,
  title={The cost of a global tariff war: A sufficient statistics approach},
  author={Lashkaripour, Ahmad},
  journal={Journal of International Economics},
  volume={131},
  pages={103419},
  year={2021},
  publisher={Elsevier},
  doi={10.1016/j.jinteco.2020.103419}
}
```

## License

The visualization code in this repository is released under the [MIT License](LICENSE). The underlying research data and methodology are from the published paper; please cite the paper when referencing the results.
