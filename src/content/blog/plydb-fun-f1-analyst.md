---
date: "2026-03-16"
draft: false
title: "Fun with PlyDB: Ask an AI Anything About F1 — In Plain English"
description:
  "Introducing F1 Analyst: point PlyDB at a season's worth of Formula 1 data and
  ask your AI agent anything — lap times, tyre strategy, telemetry, weather — no
  SQL required."
---

F1 produces more data per race than almost any other sport on the planet. Lap
times measured to the millisecond. Tyre compounds and stint lengths. Sector
splits. GPS coordinates sampled ten times a second. Weather logged throughout
every session.

It's all there. The frustrating part has always been getting to it.

Until now you needed Python, SQL, and enough patience to wrangle nested
DataFrames just to answer a simple question. We thought there was a better way —
so we built
**[F1 Analyst](https://github.com/kineticloom/plydb-fun-f1-analyst)**.

---

## What Is F1 Analyst?

F1 Analyst connects [FastF1](https://docs.fastf1.dev/) — the community library
that pulls official F1 timing data — to [PlyDB](https://www.plydb.com/), so your
AI agent can query laps, results, telemetry, tyre strategy, and weather data in
real time, in response to plain-English questions.

The idea is simple: you ask, the AI figures out the SQL, PlyDB runs it, you get
an answer. No pipelines. No cloud. No data wrangling.

It covers every race, qualifying session, and practice session from 2018
onwards.

---

## What Makes This Fun

The real joy is the questions you'd never bother with if answering them required
writing code.

**Who's on championship pace right now?** Find every season where the eventual
champion sat in their current points position after the same number of rounds.
History doesn't repeat exactly, but the early-season signal is stronger than
most people think — and the exceptions are just as interesting as the pattern.

**New regulations, new pecking order?** The 2026 season brought the most
sweeping rule changes in years. Ask the AI to rank every previous
regulation-change season by how dramatically the constructor standings shifted
from the prior year, then track whether the gaps between teams narrowed or
widened as those seasons progressed. Which team profile tends to close the gap
fastest once development gets going?

**How is your driver settling in?** Several drivers switched teams over the
winter. Ask the AI to pull their lap time deltas and qualifying gaps at circuits
they've visited before, across their previous teams versus their current one.
Already at their historical baseline? Still finding their feet? The data will
tell you.

**Did the fastest car always win the championship?** Compare each constructor's
average qualifying position — a clean proxy for raw pace — against their final
points standing, season by season. In some years the gap is stark. In others,
strategy, reliability, and driver talent tell a completely different story.

**Which circuits punish qualifying pace the most?** Look at the delta between a
driver's qualifying position and their finishing position, across every race at
every track. Some venues consistently shuffle the order; others don't. Which
ones, and why?

**What's the real cost of a safety car?** Pit stop windows compress. Strategy
calls get forced. Ask the AI to find every safety car period in a season and
compare the finishing order to where drivers would have ended up on pure pace.

**Is there a "tyre cliff" in the data?** Ask the AI to plot lap time degradation
by compound and stint length across different circuits. When does the cliff
actually arrive, and does it show up in the numbers before teams react to it?

**Rain and chaos:** Which sessions had rainfall, and by how much did lap times
swing? Which drivers consistently perform above their season average in mixed
conditions?

**Telemetry deep dives:** At Monaco, what percentage of the lap are drivers at
full throttle? Compare that to Monza. Ask your agent to find the corner where
VER carries the most minimum speed — then ask the same question about Hamilton
at his peak.

The data supports all of it. The AI does the legwork.

---

## Try It Yourself

The [repo](https://github.com/kineticloom/plydb-fun-f1-analyst) has everything
you need: a data download script, a pre-configured PlyDB setup, and a semantic
overlay so your agent understands F1-specific encodings and table relationships
out of the box.

Setup takes a few minutes. The rabbit holes take considerably longer.

---

Did you know? PlyDB can connect your AI to boring data too!

Whether it's business data in a dusty Excel sheet or a complex DevOps log in S3,
AI can be surprisingly good at making sense of a mess. PlyDB acts as the bridge,
letting your AI query across Postgres, MySQL, CSV, Excel, Parquet, Google
Sheets, and more — locally or in the cloud.

Open source and free.
[Give PlyDB a spin!](https://www.plydb.com/docs/quickstart/)
