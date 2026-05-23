<p align="center">
  <img src="https://img.shields.io/badge/Python-3.10%2B-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python">
  <img src="https://img.shields.io/badge/Pandas-Data%20Analysis-150458?style=for-the-badge&logo=pandas&logoColor=white" alt="Pandas">
  <img src="https://img.shields.io/badge/scikit--learn-ML%20Models-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white" alt="scikit-learn">
  <img src="https://img.shields.io/badge/Data-CFBD%20API-6DB33F?style=for-the-badge&logo=api&logoColor=white" alt="CFBD API">
  <img src="https://img.shields.io/badge/License-MIT-blue?style=for-the-badge" alt="MIT License">
</p>

<h1 align="center">LSU Football Analytics (2015-2025)</h1>

<p align="center">
  <strong>A 6-notebook data science project analyzing LSU Tigers football across betting markets, coaching eras, opponent-adjusted performance, recruiting pipelines, and in-game win probability - built on 141 games and 25,094 plays from the College Football Data API.</strong>
</p>

<p align="center">
  <em>No ESPN+ subscription. No proprietary scouting data. Just a free API and scikit-learn.</em>
</p>

---

## Why This Exists

College football analysis is dominated by hot takes and narrative. This project replaces opinion with data. It asks five concrete questions about LSU football and answers each one with a dedicated notebook, building from simple EDA up to a gradient boosting win probability model.

The through-line across all five analyses is **coaching alpha** - the gap between what a team's talent and schedule predict and what actually happens on the field. Two-thirds of performance variance is unexplained by recruiting inputs. That unexplained portion is where coaching lives, and this project measures it.

---

## Key Findings

| Analysis | Finding |
|----------|---------|
| **Betting (Project 1)** | LSU covers 52.2% ATS but cumulative P&L is -0.6 units (-0.4% ROI) after juice |
| **Coaching Eras (Project 2)** | Win rates nearly identical (71-75%) but trajectories differ wildly. Orgeron had extreme volatility (SP+ 33.1 peak to 4.1 trough) while Kelly is stable |
| **Opponent-Adjusted (Project 3)** | Kelly is the only coach generating sustained alpha: +3.39 ppg Margin Over Expected |
| **Recruiting (Project 4)** | Talent explains only 17% of SP+ variance. Two-thirds of performance is unexplained by inputs |
| **Win Probability (Project 5)** | Orgeron systematically lost close games (-0.141 clutch delta), Kelly systematically wins them (+0.093) |

**The unified narrative:** The betting market misprices LSU during coaching transitions because it is slow to adjust to regime changes in clutch performance. Orgeron's era had elite talent but lost toss-ups; Kelly wins them. This is the source of his +3.39 coaching alpha that the market has not fully priced in.

---

## Notebooks

| # | Notebook | Question | Method |
|---|----------|----------|--------|
| 0 | `project_0_lsu_football_eda.ipynb` | What does 10 seasons of LSU football look like? | EDA, scoring distributions, baseline ML |
| 1 | `project_1_lsu_betting_analysis.ipynb` | Is LSU consistently over- or undervalued by betting markets? | ATS records, cumulative P&L, market inefficiencies |
| 2 | `project_2_lsu_coaching_eras.ipynb` | How do Miles, Orgeron, and Kelly compare? | SP+ ratings, PPA, recruiting composites, Kruskal-Wallis |
| 3 | `project_3_lsu_opponent_adjusted.ipynb` | What happens when you control for schedule strength? | Margin Over Expected, opponent-tier analysis |
| 4 | `project_4_lsu_recruiting_performance.ipynb` | Does recruiting talent predict on-field success? | Lagged regression, transfer portal, NFL draft output |
| 5 | `project_5_lsu_win_probability.ipynb` | Can we model real-time win probability and measure clutch? | Gradient boosting (83.5% acc, 0.906 AUC), clutch delta |

---

## Quick Start

### 1. Clone and install

```bash
git clone https://github.com/lenamonj/lsu-football.git
cd lsu-football
pip install -r requirements.txt
```

### 2. Set your API key

Register for a free key at [collegefootballdata.com](https://collegefootballdata.com/key).

```bash
export CFBD_API_KEY="your-api-key-here"
```

Or on Windows PowerShell:

```powershell
$env:CFBD_API_KEY = "your-api-key-here"
```

### 3. Run

```bash
jupyter notebook
```

Notebooks are numbered 0-5 and designed to run in order, though each is self-contained with its own API calls.

---

## Data Source

All data comes from the [College Football Data API](https://collegefootballdata.com/) (CFBD). The free tier is sufficient.

| Endpoint | Data |
|----------|------|
| `/games` | Game results and box scores |
| `/betting/lines` | Historical betting lines across sportsbooks |
| `/ratings/sp` | SP+ advanced team ratings |
| `/ratings/srs` | Simple Rating System |
| `/recruiting/teams` | Team-level recruiting rankings |
| `/talent` | 247Sports talent composite |
| `/stats/season/advanced` | PPA, success rates, advanced team stats |
| `/player/returning` | Returning production metrics |
| `/draft/picks` | NFL draft selections |
| `/play/stats` | Play-by-play data for win probability |

---

## Design Decisions

- **Temporal integrity** - All models respect time. No future data leaks into historical analysis. Recruiting is lagged 1-3 years because that is when those players actually contribute.
- **Margin Over Expected (MOE)** - Raw win-loss records are misleading without schedule context. MOE = actual margin - SP+ predicted differential isolates coaching effect from talent and schedule noise.
- **Gradient boosting over logistic regression** - The win probability model uses GBM (83.5% accuracy, Brier 0.112) because score-time interactions are inherently nonlinear. Logistic regression baseline included for comparison (78.8%).
- **Kruskal-Wallis over ANOVA** - Game margins are not normally distributed. Nonparametric tests are the correct choice for comparing coaching eras.
- **Clutch delta as the key metric** - Win rate in close Q3-Q4 situations minus model-predicted win rate. This is the signal that connects the betting analysis to the coaching analysis and explains where alpha actually comes from.

---

## Project Structure

```
lsu-football/
|-- README.md
|-- LICENSE
|-- requirements.txt
|-- .gitignore
|
|-- project_0_lsu_football_eda.ipynb           # Exploratory analysis, baseline ML
|-- project_1_lsu_betting_analysis.ipynb       # ATS performance, over/under, P&L
|-- project_2_lsu_coaching_eras.ipynb          # Miles vs Orgeron vs Kelly
|-- project_3_lsu_opponent_adjusted.ipynb      # Schedule-controlled MOE
|-- project_4_lsu_recruiting_performance.ipynb # Talent pipeline, portal, draft
|-- project_5_lsu_win_probability.ipynb        # In-game GBM model, clutch analysis
|
|-- lsu_ats_by_season.png                      # + 15 more visualization outputs
+-- lsu_luck_analysis.png
```

---

## Dependencies

| Package | Purpose |
|---------|---------|
| `pandas` | Data manipulation and time series |
| `numpy` | Numerical computation |
| `matplotlib` | Visualization |
| `seaborn` | Statistical plots |
| `scikit-learn` | Gradient boosting, logistic regression, evaluation metrics |
| `scipy` | Kruskal-Wallis, Mann-Whitney U statistical tests |
| `cfbd` | College Football Data API Python client |
| `requests` | HTTP requests to CFBD API |

---

## License

This project is licensed under the [MIT License](LICENSE).

---

<p align="center">
  <sub>Built with Python and the College Football Data API. No proprietary scouting tools required.</sub>
</p>
