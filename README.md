# Does Payroll Buy Wins? MLB 2026 Payroll, Wins & a Salary Cap
 
Project by Asher Ring & Elliot Asquith
 
## The Question
 
MLB is the only major U.S. league with no salary cap or salary floor, and both are on the table in the 2026 labor talks. We asked two questions:
 
1. **Does spending more buy more wins?**
2. **Would a salary cap or floor change who wins?**
## Headline Finding
 
Money helps, but only a little. Across the 30 teams in 2026, payroll explains about **9%** of the differences in win % (r = 0.30, p = 0.11), and the trend line adds only about **4 wins per extra \$100M**. MLB's proposed \$258.2M cap and \$180.2M floor would move about \$1.2 billion between teams, but would change any team's expected wins by **4.5 or fewer**. Spending does raise the floor: none of the seven biggest payrolls finished below .457, and the four worst teams all spent \$190.2M or less.
 
## Data
 
| File | Rows | What it holds | Source |
|---|---|---|---|
| `data/mlb_team_payroll_totals.csv` | 30 | One row per team: record, average age, total payroll split into active, injured, suspended, personal leave, minor league, dead money and retained money | [Spotrac](https://www.spotrac.com/mlb/payroll/) |
| `data/mlb_player_salaries.csv` | 2,031 | One row per contract line, with full-season salary and the share the team actually paid | Spotrac team cap pages |
| `data/mlb_standings_2026_final.csv` | 30 | Final 2026 regular-season records | `pybaseball` (Baseball-Reference) |
| `data/data_issues_log.csv` | 40 | Oddities in Spotrac's own data (blank amounts, credits, repeated lines, deferred money) | Created by notebook 01 |
 
## Cleaning Steps
 
- Scraped every table on each team's Spotrac cap page, not just the active roster, so each team's payroll parts add up to Spotrac's total **exactly**.
- Gave every team the same three-letter code so the payroll and standings tables join cleanly ("Tampa Bay Rays" → `TB`).
- Checked all 30 records against the official final standings (all match; NYY and BAL played 161 games).
- Kept Spotrac's numbers as published and logged the oddities instead of editing them.
## Approach
 
| Notebook | What it does |
|---|---|
| `01_scrape_spotrac_payrolls.ipynb` | Scrapes Spotrac, builds the team and player tables, checks the totals, saves the CSVs and the issues log |
| `02_pull_final_standings.ipynb` | Pulls the final standings with `pybaseball` and cross-checks every record |
| `03_data_quality_checks.ipynb` | Confirms the data can be trusted and explains the salaries below the \$780K minimum |
| `04_payroll_vs_wins.ipynb` | Regression of win % on payroll, over- and underachievers, cost per win |
| `05_inside_the_payroll.ipynb` | How top-heavy salaries are, pay by position, money paid to players no longer on the team |
| `06_salary_cap_and_floor.ipynb` | Applies MLB's proposed cap and floor, compares proposals, estimates the change in wins |
 
## What Stands Out
 
- **Milwaukee** won 103 games on a \$145.9M payroll, 23.5 more than its payroll predicts. **Tampa Bay** won 98 with the fourth-lowest payroll.
- **The Mets** had the highest payroll (\$345.2M) and won 74, paying \$4.66M per win, 5.4× what Cleveland paid.
- The top 10% of active players earn **54%** of active-roster salary; the median player earns **\$810,000**.
- Teams paid **\$841M** to players who were no longer on the roster (dead money plus retained salary).
## Caveats
 
- **One season, 30 teams.** Too small to measure payroll's effect precisely.
- **Our payroll is not MLB's cap measure.** MLB counts payroll its luxury-tax way (average contract value plus benefits), so MLB says 8 teams are over its cap and 12 under its floor; we count 6 and 15.
- **Listed is not paid.** Ohtani's \$70M is mostly deferred to later years.
- **The cap/floor what-if assumes** money turns into wins at the 2026 rate. It is a rough estimate, not a prediction.
## Repository Structure
 
```
MLB-Payroll-vs-Wins/
├── 01_scrape_spotrac_payrolls.ipynb
├── 02_pull_final_standings.ipynb
├── 03_data_quality_checks.ipynb
├── 04_payroll_vs_wins.ipynb
├── 05_inside_the_payroll.ipynb
├── 06_salary_cap_and_floor.ipynb
├── data/
│   ├── mlb_team_payroll_totals.csv
│   ├── mlb_player_salaries.csv
│   ├── mlb_standings_2026_final.csv
│   └── data_issues_log.csv
├── images/
│   ├── team_payrolls.png
│   ├── payroll_vs_win_pct.png
│   ├── cost_per_win.png
│   ├── active_salary_distribution.png
│   └── cap_and_floor_effect.png
├── requirements.txt
└── README.md
```
 
## Reproduce It
 
```bash
pip install -r requirements.txt
```
 
Then run the notebooks in order from the project folder. Notebooks 03–06 only need the CSVs in `data/`. Notebooks 01 and 02 download fresh data (01 takes about 3 minutes because of Spotrac's 5-second crawl delay).
 
## Tools
 
Python · pandas · NumPy · matplotlib · statsmodels · SciPy · requests · Beautiful Soup · pybaseball · Jupyter
