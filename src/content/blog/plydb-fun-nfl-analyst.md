---
date: "2026-05-05"
draft: false
title: "NFL Data Analysis with AI: Ask Football Questions in Plain English"
description:
  "Analyze NFL play-by-play, NextGen Stats, PFR advanced metrics, and draft data
  with AI — no SQL required. NFL Analyst connects nflverse to PlyDB so you can
  ask your AI agent anything about football from 1999 onwards."
---

Football has always generated data. Box scores. Snap counts. Drive summaries.
Then came the analytics era — and suddenly there were EPA values on every play,
Next Gen Stats tracking receiver separation to the tenth of a yard, and PFR
columns measuring how often a quarterback was hurried, hit, or sacked before
the throw. All of it publicly available. Almost none of it easy to combine.

The data is everywhere. The friction is in the questions — the ones that require
joining play-by-play EPA to NextGen receiver separation to snap counts just to
get a single answer. Until now that meant SQL joins across a half-dozen tables
and an afternoon of data wrangling before you could even start. We thought there
was a better way — so we built
**[NFL Analyst](https://github.com/kineticloom/plydb-fun-nfl-analyst)**:
an open-source NFL analytics tool that lets anyone — fan, analyst, or developer
— explore football data through plain-English questions, no SQL required.

---

Demo with Claude

<video controls muted autoplay loop playsinline>
  <source src="/videos/plydb-nfl-analyst-demo.mp4" type="video/mp4">
</video>

---

## What Is NFL Analyst?

**NFL Analyst is an open-source NFL data analysis tool that connects your AI
agent to real football data so you can ask questions in plain English and get
answers instantly — no SQL, no Python, no data wrangling.**

{{< nfl-analyst-arch >}}

Under the hood, it connects [nflverse](https://github.com/nflverse/nflverse-data)
— the community-maintained repository that provides NFL data as ready-to-query
Parquet files — to [PlyDB](https://www.plydb.com/), so your AI agent can query
play-by-play EPA, NextGen Stats, PFR advanced metrics, rosters, snap counts,
draft picks, combine results, and a cross-system player ID register, all at
once, in response to plain-English questions.

You ask. The AI figures out the SQL. PlyDB runs it against local Parquet files.
You get an answer. No warehouse, no ETL, no cloud.

It covers play-by-play data from 1999 onwards, NextGen Stats from 2016, PFR
advanced metrics from 2018, combine data going back decades, and draft picks
from 1936.

{{< nfl-analyst-datasources >}}

**Who is this for?** NFL fans who want deeper analysis than a box score,
fantasy players hunting for edge, sabermetrics-adjacent analysts who want to
apply the same rigor to football, and developers who want a working example of
AI-powered sports analytics with PlyDB.

## What Makes This Fun

The real joy is asking the questions you'd never bother with if answering them
required writing code.

**Who are the clutch quarterbacks?** It's easy to sort by season passer rating
— but ask the agent to compare each QB's EPA per play and win probability added
in close games (score within 7 in the fourth quarter) versus blowouts over the
last three seasons. The gap between clutch performers and garbage-time stat
padders is larger than most people expect, and the list of names is surprising.

**The fourth-down revolution:** Analytics-era coaches have dramatically shifted
NFL strategy on fourth down. Ask the agent to track fourth-down attempt rates by
head coach and season, then cross-reference conversion success and downstream
win probability impact. Which coaches are driving the movement — and does
actually going for it more translate to wins, or does it just feel like it does?

**Did home-field advantage survive 2020?** The 2020 season was a natural
experiment: no fans, near-empty stadiums. Ask the agent to compare home win
percentage and point spreads before 2020, during the pandemic season, and from
2021 onward by venue type (dome vs. outdoor). The crowd effect is real — but it
is not uniform across every stadium.

**Open but underutilized receivers:** Join NextGen Stats receiving
(avg_separation) with offensive player stats (target_share, air_yards) and snap
counts. Which wide receivers consistently create the most separation from
defenders yet see a below-average share of their team's targets? These are the
names to circle before the next contract cycle.

**Quarterback performance under pressure:** Join PFR advanced passing stats
(pressure_pct, times_pressured) with play-by-play EPA. Which quarterbacks
maintain the highest EPA per play when they are pressured — and which ones fall
apart? Does pressure tolerance in one season predict performance trajectory in
the next?

**The draft value curve:** Using draft picks with career approximate value
(w_av), chart expected production by overall pick number and position. Then join
combine results to find which athletic testing metrics — 40-yard dash, vertical,
three-cone — actually predict NFL success at each position versus which combine
workouts are expensive noise.

**Snap efficiency: hidden breakout candidates:** Identify skill players who
produce high EPA per snap on limited playing time. Join snap_counts with
play-by-play EPA and player stats to surface receivers and running backs
generating elite efficiency in a small sample — the candidates most likely to
break out with more opportunity.

**Does running on first down still make sense?** Compare EPA per play on
first-and-ten runs versus passes across every team and season in the dataset.
Which teams deviate most from league-average run/pass balance on early downs —
and do the teams that pass more on first down consistently generate better
expected points added?

**Defensive efficiency by coverage type:** Which defenses have allowed the most
and least EPA through the air versus on the ground over the last five seasons?
Join defensive player stats with play-by-play coverage data to identify which
units are actually stopping the pass and which are just getting lucky with
interceptions.

**Quarterback aging curves:** At what age does EPA per dropback typically peak
for quarterbacks? Is there a Statcast-era equivalent for football — a shift in
how long careers last or when production declines — visible in the play-by-play
data since 2000?

The data supports all of it. The AI does the legwork.

---

## Try It Yourself

The [repo](https://github.com/kineticloom/plydb-fun-nfl-analyst) has everything
you need: a data download script, a pre-configured PlyDB setup, and a semantic
overlay so your agent understands NFL-specific encodings, ID relationships, and
table joins out of the box.

Setup takes a few minutes. The rabbit holes take considerably longer.

---

## Frequently Asked Questions

**Do I need to know SQL to use NFL Analyst?** No. Your AI agent handles all the
SQL. You ask questions in plain English; the agent translates them into queries,
runs them via PlyDB, and returns the answer.

**What NFL data is included?** NFL Analyst pulls 17 datasets from
[nflverse-data](https://github.com/nflverse/nflverse-data): play-by-play with
EPA and win probability (1999+), offensive and defensive player stats, weekly
snap counts (2012+), game schedules and results with Vegas lines and weather
(1999+), Next Gen Stats for passing, receiving, and rushing (2016+), PFR
advanced stats for passers, rushers, receivers, and defenders (2018+), NFL
Combine results, draft picks with career approximate value (1936+), and a
player ID register linking gsis_id, pfr_id, and espn_id for cross-dataset joins.

**What AI agent do I use with it?** Any PlyDB-compatible agent works. The repo
is set up for Claude Code, but the PlyDB config and semantic overlay are
compatible with any agent that supports tool use.

**Does it work without a cloud database or data warehouse?** Yes. All data is
stored locally as Parquet files and queried directly by PlyDB. No cloud account,
no ETL pipeline, no ongoing infrastructure required.

**How much disk space do I need?** Play-by-play data is roughly 15–25 MB per
season, so ten seasons runs 150–250 MB for that table alone. The other datasets
are much smaller — player stats, rosters, and snap counts are a few hundred KB
per season. Download only the seasons and datasets you need; the script skips
files that already exist so downloads are resumable.

**Is there a similar project for other sports or events?**
Yes — check out [Baseball Analyst](../plydb-fun-baseball-analyst) for MLB
Statcast and FanGraphs data, [F1 Analyst](../plydb-fun-f1-analyst) for Formula
1 telemetry and timing data, [Olympics Analyst](../plydb-fun-olympics-2026) for
the 2026 Winter Games medal analysis, and
[Oscars Analyst](../plydb-fun-oscars-2026) for 97 years of Academy Awards
history.

---

Did you know? PlyDB can connect your AI to boring data too!

Whether it's business data in a dusty Excel sheet or a complex DevOps log in S3,
AI can be surprisingly good at making sense of a mess. PlyDB acts as the bridge,
letting your AI query across Postgres, MySQL, CSV, Excel, Parquet, Google
Sheets, and more — locally or in the cloud.

Open source and free.
[Give PlyDB a spin!](https://www.plydb.com/docs/quickstart/)
