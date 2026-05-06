---
date: "2026-05-06"
draft: false
title: "NBA Data Analysis with AI: Ask Basketball Questions in Plain English"
description:
  "Analyze NBA box scores, advanced metrics, and play-by-play data with AI —
  no SQL required. NBA Analyst connects historical NBA data to PlyDB so you can
  ask your AI agent anything about basketball from 1946 onwards."
---

Basketball has always generated data. Box scores. Shooting percentages. Rebounding
rates. Then came the analytics revolution — and suddenly there were offensive ratings
per 100 possessions, true shooting percentages that account for every point-scoring
method, usage rates measuring exactly how much of the offense ran through each player,
and play-by-play records documenting every shot, foul, and turnover since 1996. All
of it sourced from NBA.com. Almost none of it easy to combine.

The data is everywhere. The friction is in the questions — the ones that require
joining game-by-game box scores to advanced efficiency metrics to biographical draft
data just to get a single answer. Until now that meant SQL joins across a half-dozen
tables and an afternoon of data wrangling before you could even start. We thought
there was a better way — so we built
**[NBA Analyst](https://github.com/kineticloom/plydb-fun-nba-analyst)**:
an open-source NBA analytics tool that lets anyone — fan, analyst, or developer
— explore basketball data through plain-English questions, no SQL required.

---

Demo with Claude

<video controls muted autoplay loop playsinline>
  <source src="/videos/plydb-nba-analyst-demo.mp4" type="video/mp4">
</video>

---

## What Is NBA Analyst?

**NBA Analyst is an open-source NBA data analysis tool that connects your AI agent
to real basketball data so you can ask questions in plain English and get answers
instantly — no SQL, no Python, no data wrangling.**

{{< nba-analyst-arch >}}

Under the hood, it connects a
[Kaggle dataset](https://www.kaggle.com/datasets/eoinamoore/historical-nba-data-and-player-box-scores)
sourced from NBA.com — covering every game from 1946 to the present — to
[PlyDB](https://www.plydb.com/), so your AI agent can query traditional and
advanced box scores, play-by-play events, game results, player biographies, and
franchise history, all at once, in response to plain-English questions.

You ask. The AI figures out the SQL. PlyDB runs it against local CSV and Parquet
files. You get an answer. No warehouse, no ETL, no cloud.

It covers traditional box scores from 1947, advanced per-game metrics from 1996,
play-by-play detail from 1996 (18.7 million rows), and franchise history going back
to the Basketball Association of America in 1946.

{{< nba-analyst-datasources >}}

**Who is this for?** NBA fans who want deeper analysis than a box score, fantasy
basketball players hunting for edge, sabermetrics-adjacent analysts who want to apply
statistical rigor to basketball, and developers who want a working example of
AI-powered sports analytics with PlyDB.

## What Makes This Fun

The real joy is asking the questions you'd never bother with if answering them
required writing code.

**The greatest scorers, adjusted for pace:** It's easy to sort by career points per
game — but ask the agent to weight each player's scoring output by the pace of the
era they played in. Players in the run-and-gun 1960s scored in a very different
environment than players in the defensive-grind 1990s. Adjust for pace across every
era since 1947 and a more honest all-time scoring ranking emerges — one that reorders
the leaderboard in ways most fans wouldn't predict.

**The three-point revolution, play by play:** Track the share of field goal attempts
that were three-pointers (threePointersAttempted / fieldGoalsAttempted) season by
season since 1980. The numbers tell a slow-build story: from roughly 3% in 1980 to
the Steph Curry inflection point in 2015-16, to today's 40%+ league-wide rate. Then
find the earliest team-level adopters — the franchises that ran three-heavy offenses
a full decade before the rest of the league caught on.

**Usage versus efficiency at scale:** Plot usage rate against true shooting percentage
for every player season with a usage rate above 30% since 2010. Which superstars
combine elite ball dominance with elite efficiency — and which high-usage players are
quietly hurting their teams? The cluster of names in the top-right corner is shorter
than the conventional star rankings suggest.

**Clutch performers:** Using play-by-play data, find players with the most points
scored in the final two minutes of games decided by five or fewer points. Then check
whether measured clutch performance in the regular season predicts playoff success the
following year, or whether "clutch" is just random variation in a small sample
dressing itself up as a skill.

**Home-court advantage through the COVID era:** Compare home win percentages before
2020, during the empty-arena 2020-21 season, and post-pandemic. Did home-court
advantage fully return, or did two years of playing in silence permanently recalibrate
what teams can rely on when they tip off at home?

**The shot location revolution:** Using play-by-play shot coordinates, track how the
distribution of shot attempts across court zones has shifted since 1996 — paint,
mid-range, corner three, above-the-break three. The mid-range shot's decline is
documented point by point, foot by foot. Which teams completed the transition earliest,
and which franchises still cling to the pull-up jumper at a rate that costs them
points on every possession?

**Franchise cornerstone or journeyman?** Using team histories and player stats, find
players who played at least 80% of their career games with a single franchise. How do
their career efficiency and longevity numbers compare to players who moved frequently
chasing rings? Are the one-franchise players systematically better or worse in the
advanced metrics, or does the data just tell us they had more stable front offices?

**Load management and playoff performance:** Identify star players with unusually low
regular-season games-played counts in the last decade. Do teams that rest their stars
more aggressively in the regular season perform better or worse in the playoffs? Is
load management a competitive edge or a false economy — and does the answer differ for
first-round exits versus deep playoff runs?

**Draft class treasure map:** Rank every NBA draft class since 1985 by aggregate
career points, rebounds, and assists from all players selected. Which classes produced
the most total talent — and which were catastrophic busts despite high expectations?
Then filter by pick position to find the rounds and slots that historically deliver
the best return on draft capital.

**Bench scoring and championship DNA:** Teams that win championships often have
exceptional bench units — or do they? Join benchPoints from team_stats with playoff
results to find whether second-unit scoring in the regular season predicts unexpected
playoff runs, or whether star-driven teams with weak benches consistently outperform
the raw depth numbers.

The data supports all of it. The AI does the legwork.

---

## Try It Yourself

The [repo](https://github.com/kineticloom/plydb-fun-nba-analyst) has everything you
need: a data download script, a pre-configured PlyDB setup, and a semantic overlay so
your agent understands NBA-specific encodings, ID relationships, and table joins out
of the box.

Setup takes a few minutes. The rabbit holes take considerably longer.

---

## Frequently Asked Questions

**Do I need to know SQL to use NBA Analyst?** No. Your AI agent handles all the SQL.
You ask questions in plain English; the agent translates them into queries, runs them
via PlyDB, and returns the answer.

**What NBA data is included?** NBA Analyst uses a
[Kaggle dataset](https://www.kaggle.com/datasets/eoinamoore/historical-nba-data-and-player-box-scores)
sourced from NBA.com, providing 10 files: traditional player and team box scores
(1947+), advanced per-game metrics for players and teams (1996+), play-by-play events
(1996+, 18.7M rows), game results with arena and attendance (1946+), player
biographical data including draft information and physical attributes, franchise
history tracking name changes and relocations, and the 2024-25 and 2025-26 season
schedules.

**What AI agent do I use with it?** Any PlyDB-compatible agent works. The repo is set
up for Claude Code, but the PlyDB config and semantic overlay are compatible with any
agent that supports tool use.

**Does it work without a cloud database or data warehouse?** Yes. All data is stored
locally as CSV and Parquet files and queried directly by PlyDB. No cloud account, no
ETL pipeline, no ongoing infrastructure required.

**How much disk space do I need?** The full dataset is roughly 1.8 GB. The two large
files are PlayByPlay.parquet (~900 MB) and PlayerStatisticsExtended.csv (~430 MB).
For a fast start, download only PlayerStatistics.csv (~370 MB), Games.csv (~11 MB),
and Players.csv (~500 KB) — this covers all traditional stats from 1947 and is enough
for most historical questions. The download script skips files that already exist, so
downloads are resumable and you can add more data incrementally.

**Is there a similar project for other sports or events?**
Yes — check out [NFL Analyst](../plydb-fun-nfl-analyst) for NFL play-by-play and
NextGen Stats, [Baseball Analyst](../plydb-fun-baseball-analyst) for MLB Statcast and
FanGraphs data, [F1 Analyst](../plydb-fun-f1-analyst) for Formula 1 telemetry and
timing data, [Olympics Analyst](../plydb-fun-olympics-2026) for the 2026 Winter Games
medal analysis, and [Oscars Analyst](../plydb-fun-oscars-2026) for 97 years of
Academy Awards history.

---

Did you know? PlyDB can connect your AI to boring data too!

Whether it's business data in a dusty Excel sheet or a complex DevOps log in S3,
AI can be surprisingly good at making sense of a mess. PlyDB acts as the bridge,
letting your AI query across Postgres, MySQL, CSV, Excel, Parquet, Google
Sheets, and more — locally or in the cloud.

Open source and free.
[Give PlyDB a spin!](https://www.plydb.com/docs/quickstart/)
