---
date: "2026-03-26"
draft: false
title: "MLB Data Analysis with AI: Ask Baseball Questions in Plain English"
description:
  "Analyze MLB Statcast and FanGraphs data with AI — no SQL required. Baseball
  Analyst connects pybaseball to PlyDB so you can ask your AI agent anything
  about stolen bases, power metrics, pitching trends, and more."
---

Baseball has always been a numbers sport. For over a century, fans and analysts
have obsessed over batting averages, ERAs, and on-base percentages. Then came
the Statcast era — and suddenly there were 700,000 pitches a season to work
with, each one logged with exit velocity, spin rate, launch angle, and GPS
coordinates.

The data is everywhere. The friction is in the questions — the ones that require
joining Statcast pitch outcomes to FanGraphs season stats to player IDs just to
get a single answer. Until now that meant Python, SQL, and a working knowledge
of FanGraphs column naming conventions before you could even start. We thought
there was a better way — so we built
**[Baseball Analyst](https://github.com/kineticloom/plydb-fun-baseball-analyst)**:
an open-source baseball analytics tool that lets anyone — fan, analyst, or
developer — explore MLB data through plain-English questions, no SQL required.

---

Demo with Claude

<video controls muted autoplay loop playsinline>
  <source src="/videos/plydb-baseball-analyst-demo.mp4" type="video/mp4">
</video>

---

## What Is Baseball Analyst?

**Baseball Analyst is an open-source MLB data analysis tool that connects your
AI agent to real baseball data so you can ask questions in plain English and get
answers instantly — no SQL, no Python, no data wrangling.**

Under the hood, it connects [pybaseball](https://github.com/jldbc/pybaseball) —
the community library that pulls data from Baseball Savant, FanGraphs, and
Baseball Reference — to [PlyDB](https://www.plydb.com/), so your AI agent can
query pitch-level Statcast data, season batting and pitching stats, standings,
schedules, and a cross-system player ID register, all at once, in response to
plain-English questions.

You ask. The AI figures out the SQL. PlyDB runs it against local Parquet files.
You get an answer. No warehouse, no ETL, no cloud.

It covers Statcast data from 2008 onwards, with FanGraphs and Baseball Reference
stats going back further.

**Who is this for?** Baseball fans who want deeper analysis than a box score,
sabermetrics enthusiasts who are tired of writing one-off scripts, and
developers who want a working example of AI-powered sports analytics with PlyDB.

## What Makes This Fun

The real joy is asking the questions you'd never bother with if answering them
required writing code.

**Who's actually stealing bases efficiently?** It's easy to sort by stolen base
totals — but ask the agent to rank players by success percentage with a minimum
attempt threshold, then layer in market value, and you start seeing something
interesting. Chandler Simpson and Oneil Cruz are delivering elite stolen base
production at a fraction of the cost of peers like Bobby Witt Jr. or Trea
Turner. The data finds these inefficiencies instantly.

**Does raw power actually translate to team value?** ISO (isolated power) and
wRC+ correlate at r=0.73 across qualified hitters — strong, but far from
perfect. Ask the agent to compute the residual and you immediately surface two
groups: players whose power compounds into run creation (Ronald Acuña Jr.,
George Springer — elite OBP amplifies every extra-base hit), and players whose
power leaks away (Eugenio Suarez hits 49 home runs but carries a .298 OBP; Jo
Adell has genuine pop but a 26% strikeout rate swallows it). One query, and the
whole story is visible.

**Who actually wins the trade deadline?** Using the game-by-game schedule,
compare each team's win percentage and run differential before and after July 31
each season. Which franchises consistently improve after deadline acquisitions,
and which ones add payroll without moving the needle?

**Pitch mix evolution:** How has the league-wide share of four-seam fastballs
changed since 2015? Which starters have most dramatically shifted their arsenal
— and did the change correlate with better or worse outcomes?

**The platoon advantage by pitch type:** Does the left-on-left matchup advantage
hold equally for curveballs and sliders? Pull wOBA splits by batter/pitcher
handedness and pitch type across the Statcast era.

**Which parks are most pitcher-friendly right now?** Compare barrel rate allowed
and average xwOBA when pitchers are at home vs. away over the last three
seasons. Which ballparks consistently suppress or inflate batted ball quality
beyond what the season stats suggest?

**The aging curve:** At what age does offensive production (wRC+) peak for
position players, and how does that compare to pitching performance (FIP) for
starters vs. relievers? Does the Statcast era show a different curve than the
pre-launch-angle era?

**Prospect to superstar:** Using the player ID table to join Statcast and
FanGraphs data, identify the fastest rises from debut to peak wRC+ or FIP. Which
teams have been best at developing young players into impact contributors within
their first three seasons?

The data supports all of it. The AI does the legwork.

---

## Try It Yourself

The [repo](https://github.com/kineticloom/plydb-fun-baseball-analyst) has
everything you need: a data download script, a pre-configured PlyDB setup, and a
semantic overlay so your agent understands baseball-specific encodings and table
relationships out of the box.

Setup takes a few minutes. The rabbit holes take considerably longer.

---

## Frequently Asked Questions

**Do I need to know SQL to use Baseball Analyst?** No. Your AI agent handles all
the SQL. You ask questions in plain English; the agent translates them into
queries, runs them via PlyDB, and returns the answer.

**What baseball data is included?** Baseball Analyst pulls from three sources
via pybaseball: pitch-level Statcast data from Baseball Savant (2008+), advanced
batting and pitching statistics from FanGraphs, standings and schedules from
Baseball Reference, and the Chadwick Bureau player ID register for cross-dataset
joins.

**What AI agent do I use with it?** Any PlyDB-compatible agent works. The repo
is set up for Claude Code, but the PlyDB config and semantic overlay are
compatible with any agent that supports tool use.

**Does it work without a cloud database or data warehouse?** Yes. All data is
stored locally as Parquet files and queried directly by PlyDB. No cloud account,
no ETL pipeline, no ongoing infrastructure required.

**Is there a similar project for other sports?** Yes — check out
[F1 Analyst](../plydb-fun-f1-analyst), which does the same thing for Formula 1
data: lap times, tyre strategy, telemetry, and race results, all queryable in
plain English.

---

Did you know? PlyDB can connect your AI to boring data too!

Whether it's business data in a dusty Excel sheet or a complex DevOps log in S3,
AI can be surprisingly good at making sense of a mess. PlyDB acts as the bridge,
letting your AI query across Postgres, MySQL, CSV, Excel, Parquet, Google
Sheets, and more — locally or in the cloud.

Open source and free.
[Give PlyDB a spin!](https://www.plydb.com/docs/quickstart/)
