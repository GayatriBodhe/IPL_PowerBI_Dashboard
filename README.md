# 🏏 IPL Season Analysis Dashboard (2008–2025) — Power BI

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)
![Data Analytics](https://img.shields.io/badge/Data%20Analytics-4CAF50?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)

> An interactive Power BI dashboard analysing **17 seasons of IPL data (2008–2025)** — covering match outcomes, player performances, team standings, and scoring milestones. A single season slicer dynamically updates every visual on the page.

---

## 📸 Dashboard Preview

![IPL Dashboard Screenshot](assets/dashboard_preview.png)

> *2025 Season shown — RCB Champions, Punjab Kings Runner-Up*

---

## 🎯 Project Objective

Cricket generates enormous amounts of structured data every season. The goal of this project was to transform raw IPL match and player data into a clean, executive-ready dashboard that any cricket fan, analyst, or team management stakeholder could use to instantly understand a season's story — without writing a single SQL query or scrolling through spreadsheets.

---

## 📊 Dashboard Features

### 🔵 Primary KPIs (Season-level)
| KPI | Description |
|-----|-------------|
| **Season Champion** | Winning team with dynamic logo |
| **Season Runner-Up** | Second-place team with dynamic logo |

### 🟡 Secondary KPIs (Match Overview)
| KPI | 2025 Value |
|-----|-----------|
| Total Sixes | 1,296 |
| Total Fours | 2,251 |
| Total Matches | 74 |
| Total Teams | 10 |
| Centuries | 9 |
| Half-Centuries | 143 |
| Total Venues | 14 |

### 🟠 Player Spotlight Cards
Each card shows **Player Name**, **Stat Count**, **Team Name**, and a **dynamically rendered player image**:

- 🟠 **Orange Cap** — Top run-scorer (B Sai Sudharsan — 759 runs, Gujarat Titans)
- 🟣 **Purple Cap** — Top wicket-taker (M Prasidh Krishna — 25 wickets, Gujarat Titans)
- 🏏 **Most Fours** — B Sai Sudharsan — 88 fours, Gujarat Titans
- 💥 **Most Sixes** — N Pooran — 40 sixes, Lucknow Super Giants

### 📋 Points Table
Full league standings with:
- Team Logo + Team Name
- Matches Played | Won | Lost | NR | Tie
- **Total Points** (2 pts = Win, 1 pt = NR/Tie, 0 pts = Loss)

---

## 🛠️ Technical Implementation

### Data Modelling
- Structured a star schema with a central `matches` fact table linked to `teams`, `players`, and `deliveries` dimension tables
- Built relationships enabling cross-filtering between all visuals via the season slicer

### DAX Measures Used
```dax
-- Total Sixes for selected season
Total Sixes = CALCULATE(COUNTROWS(deliveries), deliveries[batsman_runs] = 6)

-- Orange Cap Holder (most runs in season)
Orange Cap Runs = 
    MAXX(
        TOPN(1, SUMMARIZE(deliveries, deliveries[batter], "Runs", SUM(deliveries[batsman_runs])), [Runs], DESC),
        [Runs]
    )

-- Points Table calculation
Total Points = ([Matches Won] * 2) + [Tie] + [No Result]
```

### Dynamic Image Rendering
- Player and team images are loaded via **URL-based image columns** in the data model
- Conditional rendering ensures the correct player image appears per season without manual filtering

---

## 📁 Repository Structure

```
IPL-Analysis-PowerBI/
│
├── 📂 dataset/
│   ├── matches.csv           # Match-level data 2008–2025
│   ├── deliveries.csv        # Ball-by-ball delivery data
│   └── players.csv           # Player metadata + image URLs
│
├── 📂 assets/
│   └── dashboard_preview.png # Screenshot of the dashboard
│
├── 📄 IPL_Analysis.pbix      # Power BI Desktop file
└── 📄 README.md
```

---

## 📦 Data Source

- Dataset sourced from [Kaggle — IPL Complete Dataset](https://www.kaggle.com/datasets/patrickb1912/ipl-complete-dataset-20082020)
- Extended and cleaned to include 2021–2025 seasons
- Player images sourced from publicly available IPL/ESPN Cricinfo URLs

---

## 🚀 How to Use

1. Clone this repository
   ```bash
   git clone https://github.com/GayatriBodhe/IPL-Analysis-PowerBI.git
   ```
2. Open `IPL_Analysis.pbix` in **Power BI Desktop** (free download from Microsoft)
3. Use the **"Select IPL Season"** dropdown slicer at the top right to switch between any season from 2008 to 2025
4. All KPIs, player cards, and the points table update instantly

---

## 💡 Key Learnings

- Designing a **single-slicer driven dashboard** that controls 15+ visuals simultaneously
- Writing **DAX measures** for dynamic TOPN ranking (Orange Cap, Purple Cap logic)
- Implementing **conditional image rendering** in Power BI using URL-based image columns
- Building a **points table with calculated columns** matching real IPL scoring rules
- Creating **executive-ready layouts** — clean, minimal, no chart junk

---

## 🔮 Future Improvements

- [ ] Add season-over-season **trend comparisons** (e.g., average sixes per match over years)
- [ ] Integrate **venue-level heatmaps** showing team win rates by stadium
- [ ] Add a **player career tracker** showing performance across multiple seasons
- [ ] Publish to **Power BI Service** for web-based sharing

---

## 👩‍💻 About Me

**Gayatri Bodhe** — M.Sc. Computer Science student at Kaveri College, Pune  
Aspiring Data Analyst | Power BI | SQL | Python | Flutter Developer

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=flat&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/gayatri-bodhe/)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat&logo=github&logoColor=white)](https://github.com/GayatriBodhe)

---

*If you found this useful, please ⭐ star the repo — it helps others find it!*
