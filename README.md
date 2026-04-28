# 🏏 IPL Season Analysis Dashboard (2008–2025) — Power BI

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)
![Data Analytics](https://img.shields.io/badge/Data%20Analytics-4CAF50?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)

> An interactive Power BI dashboard analysing **17 seasons of IPL data (2008–2025)** — covering match outcomes, player performances, team standings, and scoring milestones. A single season slicer dynamically updates every visual on the page.

---

## 📸 Dashboard Preview

<img width="400" height="220" alt="Screen Recording 2026-04-28 105557" src="https://github.com/user-attachments/assets/f0a45374-9958-4bef-a9dd-2639143ce6ae" />

> *Video Preview for season slicer working dynamically for images, points table, players, and Champion stats

<img width="674" height="372" alt="image" src="https://github.com/user-attachments/assets/ab6def26-bb5c-48f1-b8cd-26acd1c92bc2" />


> *2025 Season shown — RCB Champions, Punjab Kings Runner-Up*

<img width="674" height="372" alt="image" src="https://github.com/user-attachments/assets/6dabfcb7-489d-4d65-83bd-1b9811c7c4ac" />


> *2023 Season shown — CSK Champions, Gujarat Titans Runner-Up*

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

### 📐 DAX Measures Reference — IPL Season Analysis Dashboard

> Complete reference of all DAX measures used in `IPL_Dashboard.pbix`
> Organised by category. All measures are season-context aware via the IPL Season slicer.

---

#### 🏆 Season Result Measures

```dax
--Season Winner = 
 VAR SelectedSeason = SELECTEDVALUE(ipl_matches_data[season])    --2025

 VAR FinalMatchDate = CALCULATE(MAX(ipl_matches_data[match_date]),
                                    ipl_matches_data[season] = SelectedSeason)  --Max Date = 3rd June

VAR FinalMatchWinner = CALCULATE(MAX(ipl_matches_data[match_winner]),  --Winner Team
                                    ipl_matches_data[season] = SelectedSeason,
                                    ipl_matches_data[match_date] = FinalMatchDate)

RETURN FinalMatchWinner 

-- Season Winner team logo (URL-based image)
SeasonWinnerLogo = 

 VAR SelectedSeason = SELECTEDVALUE(ipl_matches_data[season])    --2025

 VAR FinalMatchDate = CALCULATE(MAX(ipl_matches_data[match_date]),
                                    ipl_matches_data[season] = SelectedSeason)  --Max Date = 3rd June

VAR FinalMatchWinner = CALCULATE(MAX(ipl_matches_data[match_winner]),  --Winner Team
                                    ipl_matches_data[season] = SelectedSeason,
                                    ipl_matches_data[match_date] = FinalMatchDate)

RETURN 

LOOKUPVALUE(
    teams_data[image_url],
    teams_data[team_name], FinalMatchWinner)

-- Runner Up name
Runner Up = 

 VAR SelectedSeason = SELECTEDVALUE(ipl_matches_data[season])    --2025

 VAR FinalMatchDate = CALCULATE(MAX(ipl_matches_data[match_date]),
                                    ipl_matches_data[season] = SelectedSeason)  --Max Date = 3rd June

VAR FinalMatchWinner = CALCULATE(MAX(ipl_matches_data[match_winner]),  --Winner Team
                                    ipl_matches_data[season] = SelectedSeason,
                                    ipl_matches_data[match_date] = FinalMatchDate)

VAR Team1 = CALCULATE(MAX(ipl_matches_data[team1]),  --Team 1
                                    ipl_matches_data[season] = SelectedSeason,
                                    ipl_matches_data[match_date] = FinalMatchDate)

VAR Team2 = CALCULATE(MAX(ipl_matches_data[team2]),  --Team 2
                                    ipl_matches_data[season] = SelectedSeason,
                                    ipl_matches_data[match_date] = FinalMatchDate)

RETURN
IF(FinalMatchWinner=Team1, Team2, Team1)

-- Runner Up team logo
Runner UP Team Logo = 

 VAR SelectedSeason = SELECTEDVALUE(ipl_matches_data[season])    --2025

 VAR FinalMatchDate = CALCULATE(MAX(ipl_matches_data[match_date]),
                                    ipl_matches_data[season] = SelectedSeason)  --Max Date = 3rd June

VAR FinalMatchWinner = CALCULATE(MAX(ipl_matches_data[match_winner]),  --Winner Team
                                    ipl_matches_data[season] = SelectedSeason,
                                    ipl_matches_data[match_date] = FinalMatchDate)

VAR Team1 = CALCULATE(MAX(ipl_matches_data[team1]),  --Team 1
                                    ipl_matches_data[season] = SelectedSeason,
                                    ipl_matches_data[match_date] = FinalMatchDate)

VAR Team2 = CALCULATE(MAX(ipl_matches_data[team2]),  --Team 2
                                    ipl_matches_data[season] = SelectedSeason,
                                    ipl_matches_data[match_date] = FinalMatchDate)

RETURN
LOOKUPVALUE(teams_data[image_url], teams_data[team_name], [Runner Up])
```

---

#### 📊 Secondary KPI Measures

```dax
-- Total Matches in selected season
Total Matches = 
CALCULATE(DISTINCTCOUNT(ipl_matches_data[match_id]))

-- Total Teams in selected season
Total Teams = CALCULATE(DISTINCTCOUNT(ipl_matches_data[team1]))

-- Total Venues in selected season
Total Venues = 
CALCULATE(DISTINCTCOUNT(ipl_matches_data[venue]))

-- Total 6's in selected season
Total 6's = 

CALCULATE(COUNTROWS(ball_by_ball_data), ball_by_ball_data[batter_runs]=6,
            KEEPFILTERS(VALUES(ipl_matches_data[season])))

-- Total 4's in selected season
Totat 4's = 

CALCULATE(COUNTROWS(ball_by_ball_data), ball_by_ball_data[batter_runs]=4,
            KEEPFILTERS(VALUES(ipl_matches_data[season])))
```

---

#### 🏅 Centuries & Half-Centuries

```dax
-- Centuries (individual innings of 100+ runs)
Centuries = 

VAR SelectedSeason = SELECTEDVALUE(ipl_matches_data[season])

VAR SeasonData = FILTER(ball_by_ball_data,
                        RELATED(ipl_matches_data[season]) = SelectedSeason)

VAR BatterRuns = 
        SUMMARIZE(SeasonData, ball_by_ball_data[match_id],
            ball_by_ball_data[batter], "Total Runs", SUM(ball_by_ball_data
            [batter_runs]))

VAR CenturyCount = FILTER(BatterRuns, [Total Runs] >= 100)

RETURN COUNTROWS(CenturyCount)

-- Half Centuries (individual innings of 50-99 runs)
Half Centuries = 

VAR SelectedSeason = SELECTEDVALUE(ipl_matches_data[season])

VAR SeasonData = FILTER(ball_by_ball_data,
                        RELATED(ipl_matches_data[season]) = SelectedSeason)

VAR BatterRuns = 
        SUMMARIZE(SeasonData, ball_by_ball_data[match_id],
            ball_by_ball_data[batter], "Total Runs", SUM(ball_by_ball_data
            [batter_runs]))

VAR CenturyCount = FILTER(BatterRuns, [Total Runs] >= 50 && [Total Runs] < 100)

RETURN COUNTROWS(CenturyCount)
```

---

#### 🟠 Orange Cap Measures (Top Run Scorer)

```dax
-- Orange Cap Holder name
Orange Cap Holder = 

VAR SelectedSeason =  SELECTEDVALUE(ipl_matches_data[season])

VAR SeasonDataOnly =
                    FILTER(ball_by_ball_data, RELATED(ipl_matches_data[season]) = SelectedSeason)

VAR RunSummary = 
                SUMMARIZE(SeasonDataOnly, ball_by_ball_data[batter], "Total Runs", SUM(ball_by_ball_data[batter_runs]))

VAR MaxRun = MAXX(RunSummary, [Total Runs])

VAR TopScorer =
                CALCULATETABLE(VALUES(ball_by_ball_data[batter]), FILTER(RunSummary, [Total Runs] = MaxRun))
   RETURN MAXX(TopScorer, ball_by_ball_data[batter])    

-- Orange Cap total runs
Orange Cap Runs = 

VAR SelectedSeason =  SELECTEDVALUE(ipl_matches_data[season])

VAR SeasonDataOnly =
                    FILTER(ball_by_ball_data, RELATED(ipl_matches_data[season]) = SelectedSeason)

VAR RunSummary = 
                SUMMARIZE(SeasonDataOnly, ball_by_ball_data[batter], "Total Runs", SUM(ball_by_ball_data[batter_runs]))

VAR MaxRuns = MAXX(RunSummary, [Total Runs])

   RETURN MaxRuns
```

---

#### 🟣 Purple Cap Measures (Top Wicket Taker)

```dax
-- Purple Cap Holder name
Purple Cap Holder = 

VAR SelectedSeason = SELECTEDVALUE(ipl_matches_data[season])

--Filter: wickets in selected season, exclude non-bowler dismissals

VAR SeasonWickets = 
                    FILTER(
                        ball_by_ball_data,
                        RELATED(ipl_matches_data[season]) = SelectedSeason && 
                        ball_by_ball_data[is_wicket] = TRUE() && 
                        NOT ball_by_ball_data[wicket_kind] IN {"run out", "retired hurt", " obstructing the field", "retired out"}
                    )

--summarize bowler and count wickets

VAR WicketSummary = 
    SUMMARIZE(
        SeasonWickets, 
        ball_by_ball_data[bowler],
        "WicketCount", COUNTROWS(
        FILTER(SeasonWickets, ball_by_ball_data[bowler] = EARLIER(ball_by_ball_data[bowler]))
        )
    )

--Find highest wicket count

VAR MaxWickets = MAXX(WicketSummary, [WicketCount])

--Get the bowler(s) with that wicket count 
VAR TopBowler = 
            CALCULATETABLE(
                VALUES(ball_by_ball_data[bowler]),
                FILTER(WicketSummary, [WicketCount] = MaxWickets)
            )

--return the name(if multiple, it picks one alphabetically)
RETURN
    MAXX(TopBowler, ball_by_ball_data[bowler])


-- Purple Cap wicket count
Purple Cap Wicket Count = 

VAR SelectedSeason = SELECTEDVALUE(ipl_matches_data[season])

--step 1: Filter valid bowler wickets in selected season

VAR SeasonWickets = 
        FILTER(
            ball_by_ball_data,
            RELATED(ipl_matches_data[season]) = SelectedSeason && 
            ball_by_ball_data[is_wicket] = TRUE() && 
            NOT ball_by_ball_data[wicket_kind] IN {"run out", "retired out", "obstructing field"}
            )

--step 2: summarize wicket per bowler

VAR WicketSummary = 
        SUMMARIZE(
            SeasonWickets,
            ball_by_ball_data[bowler],
            "WicketCount", COUNTROWS(
                FILTER(
                    SeasonWickets,
                    ball_by_ball_data[bowler] = EARLIER(ball_by_ball_data[bowler])
                )
            )
        )

-- step3: get the highest wicket count 
VAR MaxWickets = MAXX(WicketSummary, [WicketCount])

RETURN MaxWickets

```

---

#### 🏏 Top Fours Measures

```dax
-- Top Fours player name
Top Fours Player name = 

VAR SelectedSeason = SELECTEDVALUE(ipl_matches_data[season])

VAR SeasonFours = FILTER(ball_by_ball_data, RELATED(ipl_matches_data[season]) = SelectedSeason && 
                        ball_by_ball_data[batter_runs] = 4) 

VAR FourSummary = SUMMARIZE(SeasonFours, ball_by_ball_data[batter], "FoursCount",
                            COUNTROWS(FILTER(SeasonFours, ball_by_ball_data[batter] = EARLIER(ball_by_ball_data[batter])
                            )
                            )
)

VAR MaxFours = MAXX(FourSummary, [FoursCount])

VAR TopFoursPlayer = CALCULATETABLE(VALUES(ball_by_ball_data[batter]),
                       FILTER(FourSummary, [FoursCount] = MaxFours))

RETURN MAXX(TopFoursPlayer, ball_by_ball_data[batter])

-- Top Fours count
Top Fours Count = 

VAR SelectedSeason = SELECTEDVALUE(ipl_matches_data[season])

VAR SeasonFours = FILTER(ball_by_ball_data, RELATED(ipl_matches_data[season]) = SelectedSeason && 
                        ball_by_ball_data[batter_runs] = 4) 

VAR FourSummary = SUMMARIZE(SeasonFours, ball_by_ball_data[batter], "FoursCount",
                            COUNTROWS(FILTER(SeasonFours, ball_by_ball_data[batter] = EARLIER(ball_by_ball_data[batter])
                            )
                            )
)

VAR MaxFours = MAXX(FourSummary, [FoursCount])

RETURN MaxFours


-- Top Fours player Team name
Top Fours PlayerTeam name = 

VAR SelectedSeason = SELECTEDVALUE(ipl_matches_data[season])

VAR SeasonFours = FILTER(ball_by_ball_data, RELATED(ipl_matches_data[season]) = SelectedSeason && 
                        ball_by_ball_data[batter_runs] = 4) 

VAR FourSummary = SUMMARIZE(SeasonFours, ball_by_ball_data[batter], "FoursCount",
                            COUNTROWS(FILTER(SeasonFours, ball_by_ball_data[batter] = EARLIER(ball_by_ball_data[batter])
                            )
                            )
)

VAR MaxFours = MAXX(FourSummary, [FoursCount])

VAR TopFoursPlayer = CALCULATETABLE(VALUES(ball_by_ball_data[batter]),
                       FILTER(FourSummary, [FoursCount] = MaxFours))

Var BatterTeam = 
                CALCULATE(MAX(ball_by_ball_data[team_batting]),
                FILTER(SeasonFours, ball_by_ball_data[batter] = MAXX(TopFoursPlayer, ball_by_ball_data[batter])))

RETURN BatterTeam
```

---

#### 💥🏏 Top Sixes Measures

```dax
-- Top Sixes player name
Top Sixes Player name = 

VAR SelectedSeason = SELECTEDVALUE(ipl_matches_data[season])

VAR SeasonSixes = FILTER(ball_by_ball_data, RELATED(ipl_matches_data[season]) = SelectedSeason && 
                        ball_by_ball_data[batter_runs] = 6) 

VAR SixesSummary = SUMMARIZE(SeasonSixes, ball_by_ball_data[batter], "SixesCount",
                            COUNTROWS(FILTER(SeasonSixes, ball_by_ball_data[batter] = EARLIER(ball_by_ball_data[batter])
                            )
                            )
)

VAR MaxSixes = MAXX(SixesSummary, [SixesCount])

VAR TopSixesPlayer = CALCULATETABLE(VALUES(ball_by_ball_data[batter]),
                       FILTER(SixesSummary, [SixesCount] = MaxSixes))

RETURN MAXX(TopSixesPlayer, ball_by_ball_data[batter])


-- Top Sixes count
Top Sixes Count = 

VAR SelectedSeason = SELECTEDVALUE(ipl_matches_data[season])

VAR SeasonSixes = FILTER(ball_by_ball_data, RELATED(ipl_matches_data[season]) = SelectedSeason && 
                        ball_by_ball_data[batter_runs] = 6) 

VAR SixesSummary = SUMMARIZE(SeasonSixes, ball_by_ball_data[batter], "SixesCount",
                            COUNTROWS(FILTER(SeasonSixes, ball_by_ball_data[batter] = EARLIER(ball_by_ball_data[batter])
                            )
                            )
)

VAR MaxSixes = MAXX(SixesSummary, [SixesCount])

RETURN MaxSixes


-- Top Sixes player team name
Top Sixes PlayerTeam name = 

VAR SelectedSeason = SELECTEDVALUE(ipl_matches_data[season])

VAR SeasonSixes = FILTER(ball_by_ball_data, RELATED(ipl_matches_data[season]) = SelectedSeason && 
                        ball_by_ball_data[batter_runs] = 6) 

VAR SixesSummary = SUMMARIZE(SeasonSixes, ball_by_ball_data[batter], "SixesCount",
                            COUNTROWS(FILTER(SeasonSixes, ball_by_ball_data[batter] = EARLIER(ball_by_ball_data[batter])
                            )
                            )
)

VAR MaxSixes = MAXX(SixesSummary, [SixesCount])

VAR TopSixesPlayer = CALCULATETABLE(VALUES(ball_by_ball_data[batter]),
                       FILTER(SixesSummary, [SixesCount] = MaxSixes))

Var BatterTeam = 
                CALCULATE(MAX(ball_by_ball_data[team_batting]),
                FILTER(SeasonSixes, ball_by_ball_data[batter] = MAXX(TopSixesPlayer, ball_by_ball_data[batter])))

RETURN BatterTeam

```

---

#### 📋 Points Table Measures

```dax
-- Matches Played per team
Matches Played = 

VAR SelectedSeason = SELECTEDVALUE(ipl_matches_data[season])

VAR Team1Matches = 
                    CALCULATE(COUNTROWS(ipl_matches_data),
                    USERELATIONSHIP(ipl_matches_data[team1], teams_data[team_name]),
                    ipl_matches_data[season] = SelectedSeason,
                    ipl_matches_data[match_type] = "T20")

VAR Team2Matches = 
                    CALCULATE(COUNTROWS(ipl_matches_data),
                    USERELATIONSHIP(ipl_matches_data[team2], teams_data[team_name]),
                    ipl_matches_data[season] = SelectedSeason,
                    ipl_matches_data[match_type] = "T20")

RETURN Team1Matches + Team2Matches

-- Matches Won per team
Matches Won = 

VAR SelectedSeason = SELECTEDVALUE(ipl_matches_data[season])

VAR CurrentTeam = SELECTEDVALUE(teams_data[team_name])

RETURN
CALCULATE(COUNTROWS(ipl_matches_data),
        ipl_matches_data[season] = SelectedSeason,
        ipl_matches_data[match_winner] = CurrentTeam,
        ipl_matches_data[match_type] = "T20")

-- Matches Lost per team
Matches Lost = 

VAR SelectedSeason = SELECTEDVALUE(ipl_matches_data[season])

VAR Team1LostMatches = 
                    CALCULATE(COUNTROWS(ipl_matches_data),
                    USERELATIONSHIP(ipl_matches_data[team1], teams_data[team_name]),
                    ipl_matches_data[season] = SelectedSeason,
                    ipl_matches_data[match_type] = "T20",
                    NOT ISBLANK(ipl_matches_data[match_winner]),
                    ipl_matches_data[match_winner] <> ipl_matches_data[team1]
                    )

VAR Team2LostMatches = 
                    CALCULATE(COUNTROWS(ipl_matches_data),
                    USERELATIONSHIP(ipl_matches_data[team2], teams_data[team_name]),
                    ipl_matches_data[season] = SelectedSeason,
                    ipl_matches_data[match_type] = "T20",
                    NOT ISBLANK(ipl_matches_data[match_winner]),
                    ipl_matches_data[match_winner] <> ipl_matches_data[team2]
                    )

RETURN Team1LostMatches + Team2LostMatches

-- No Result matches
No Result Played = 

VAR SelectedSeason = SELECTEDVALUE(ipl_matches_data[season])

VAR Team1Matches = 
                    CALCULATE(COUNTROWS(ipl_matches_data),
                    USERELATIONSHIP(ipl_matches_data[team1], teams_data[team_name]),
                    ipl_matches_data[season] = SelectedSeason,
                    ipl_matches_data[match_type] = "T20",
                    ipl_matches_data[result] = "no result"
                    )

VAR Team2Matches = 
                    CALCULATE(COUNTROWS(ipl_matches_data),
                    USERELATIONSHIP(ipl_matches_data[team2], teams_data[team_name]),
                    ipl_matches_data[season] = SelectedSeason,
                    ipl_matches_data[match_type] = "T20",
                    ipl_matches_data[result] = "no result"
)

RETURN Team1Matches + Team2Matches

-- Tie / Super Over matches
Tie Played = 

VAR SelectedSeason = SELECTEDVALUE(ipl_matches_data[season])

VAR Team1Matches = 
                    CALCULATE(COUNTROWS(ipl_matches_data),
                    USERELATIONSHIP(ipl_matches_data[team1], teams_data[team_name]),
                    ipl_matches_data[season] = SelectedSeason,
                    ipl_matches_data[match_type] = "T20",
                    ipl_matches_data[result] = "tie"
                    )

VAR Team2Matches = 
                    CALCULATE(COUNTROWS(ipl_matches_data),
                    USERELATIONSHIP(ipl_matches_data[team2], teams_data[team_name]),
                    ipl_matches_data[season] = SelectedSeason,
                    ipl_matches_data[match_type] = "T20",
                    ipl_matches_data[result] = "tie"
)

RETURN Team1Matches + Team2Matches

-- Total Points (IPL official scoring: 2 pts win, 1 pt NR/Tie, 0 pts loss)
Total Points = 

VAR Win = [Matches Won]

VAR NR = [No Result Played]

RETURN (Win*2) + NR

-- IPL Season (slicer bridge measure)
IPL Season =
    SELECTEDVALUE(ipl_matches_data[season])
```

---

## 📌 Data Model Reference

| Table | Type | Key Columns |
|-------|------|-------------|
| `ipl_matches_data` | Fact (match level) | `match_id`, `season`, `match_winner`, `match_type` |
| `ball_by_ball_data` | Fact (ball level) | `match_id`, `batter`, `bowler`, `batter_runs` |
| `players-data-updated` | Dimension | `player_name`, `player_image`, `player_id` |
| `teams_data` | Dimension | `team_name`, `team_id`, `image_url` |

**Relationships:**
- `ball_by_ball_data[match_id]` → `ipl_matches_data[match_id]` (Many-to-One)
- `players-data-updated[player_id]` → `ipl_matches_data` (One-to-Many)
- `teams_data[team_name]` → `ipl_matches_data[team1/team2/match_winner]` (One-to-Many)

*All measures are season-context aware — they filter automatically based on the IPL Season slicer selection.*

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

- Dataset sourced from https://drive.google.com/drive/folders/1OS0t10CvR_SgLB-DpqoLHCOr66dIIxO7 .
- Player images sourced from publicly available IPL/ESPN Cricinfo URLs

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
