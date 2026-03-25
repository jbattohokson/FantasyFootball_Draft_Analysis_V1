# 🏈 Fantasy Football Draft Analysis

> *"Which players deliver the most value relative to their draft position — and which positions are most predictable year over year?"*

---

## Overview

A five-year (2021–2025) PPR fantasy football analytics project covering **QB, RB, WR, and TE** positions. Using MySQL to build a unified data warehouse from raw FantasyPros season stats, and Python to run draft value scoring, consistency analysis, and tier classification — the goal is to surface which players are elite steals, which are busts, and how predictable each position is across seasons.

---

## Tools & Technologies

| Tool | Purpose |
|---|---|
| **MySQL** | Data warehouse creation, ETL from raw position tables, all 15 analysis queries |
| **Python** (pandas, NumPy, Matplotlib) | Statistical analysis, PPR tier classification, visualization, export |
| **Tableau** | Interactive player production and draft value dashboard |

---

## Data Source

- **FantasyPros historical season data** — raw per-season tables for QB, RB, WR, TE (2021–2025)
- Tables: `fantasypros_qb_2021` through `fantasypros_te_2025` (20 source tables)
- All data unified into a single `fantasy_football_combined` table via MySQL ETL pipeline

---

## Methodology

### SQL Layer (MySQL)
The SQL script builds a complete data pipeline:

1. Creates `fantasy_football_combined` with unified schema across all positions and years
2. Loads all 20 source tables with position-appropriate point calculation formulas
3. Runs 15 analysis queries covering: points by position, YoY trends, top performers, draft value, tier classification, consistency scoring, and position predictability

**Scoring Formulas:**
- **QB:** `passing_yards × 0.04 + passer_rating × 0.1` *(approximation — TD data not in source tables)*
- **WR/TE:** `receiving_yards × 0.1 + receptions` *(standard PPR receiving formula)*
- **RB:** Pre-calculated PPR column from FantasyPros source data

### Python Layer
- Loads 15 SQL query result CSVs and runs additional analysis
- Applies PPR performance tier benchmarks (Elite: 25+ pts, Starter: 20–25 pts, Flex: 12–20 pts, Below Avg: <8 pts)
- Identifies elite draft steals (high `value_gained` relative to position average)
- Flags most consistent players by lowest standard deviation
- Generates 5 charts and exports 8 analysis CSVs for Tableau

---

## PPR Performance Tiers

| Tier | Threshold | Description |
|---|---|---|
| Elite Performance | 25+ points | Must-start, weekly difference-makers |
| Solid/Starter | 20–25 points | Reliable starters in any format |
| Flex/Average | 12–20 points | Serviceable flex plays |
| Below Average | < 8 points | Bench or injured |

---

## Key Analyses

**Draft Value Scoring Model** — Each player-season is scored against position average: `pts - position_avg`. Players more than 1 std deviation above average are labeled "Elite Steal"; below average labeled "Bust".

**Position Predictability** — Uses coefficient of variation (CV = std dev / mean) to rank positions by how consistently top performers remain top performers year over year.

**Player Consistency** — Standard deviation of season fantasy points for multi-year players: lower std dev = more reliable weekly floor.

---

## Repository Contents

```
├── Fantasy_Football_Model.py          # Python analysis pipeline
├── Fantasy_Football_MYSQL.sql         # MySQL ETL + 15 analysis queries
├── FF_PPR_Player_Production_Analysis_V2.twbx  # Tableau workbook
└── SQL Code/
    ├── Result1.csv  through  Result16.csv     # SQL query outputs (loaded by Python)
```

---

## How to Run

**Step 1 — SQL (MySQL Workbench or CLI):**
```sql
-- Run Fantasy_Football_MYSQL.sql in MySQL Workbench
-- This creates the combined table, loads all data, and runs analysis queries
-- Export each query result as Result1.csv through Result15.csv to the SQL Code/ folder
```

**Step 2 — Python:**
```bash
pip install pandas numpy matplotlib

python "Python Code/Fantasy_Football_Model.py"
```

Charts saved to `Python Code/Outputs/`. Analysis CSVs also saved to `Outputs/` for Tableau use.

---

*Five seasons of PPR data: 2021–2025. All scoring is PPR format (1 point per reception).*
