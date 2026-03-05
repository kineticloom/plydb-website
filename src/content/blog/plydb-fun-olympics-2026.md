---
date: "2026-03-05"
draft: false
title:
  "Fun with PlyDB: We Asked an AI to Analyze the 2026 Winter Olympics. Here's
  What It Found."
description:
  "Using PlyDB and Claude Code to chat with Olympic data — medals, GDP, and the
  best sport to win gold if you're starting from scratch."
---

Brazil won alpine skiing gold. One man swept every men's cross-country event.
And the Netherlands — a famously flat country — finished third in the medal
table.

The Milano-Cortina 2026 Winter Olympics delivered surprises worth digging into.
So we did what any reasonable data nerd would do: we pointed
[PlyDB](https://www.plydb.com/) and
[Claude Code](https://claude.com/product/claude-code) at the raw Olympic
datasets and asked questions in plain English.

The results were more fun than we expected.

---

## What is PlyDB, in one sentence?

PlyDB is an open-source tool that lets AI agents query your data directly — no
pipelines, no data wrangling, no ETL. You point it at your files (CSV, Parquet,
Excel, databases, [whatever](https://www.plydb.com/docs/data-sources/)), and
your AI can ask SQL questions against them in real time.

For this project, we connected PlyDB to five Kaggle datasets: athletes, coaches,
medals, schedules, venues, plus World Bank GDP data and country latitude
coordinates. Then we just... started chatting.

---

## Finding 1: Norway Is in a Different League

Norway sent 80 athletes to Milano-Cortina and came home with **41 medals** — 18
of them gold. That's a 51% medal rate. One in two Norwegian athletes stood on a
podium.

Johannes Hoesflot Klaebo alone won **6 gold medals** — every single men's
cross-country skiing event. All six. The man did not lose once.

Norway also swept Nordic Combined entirely (3 events, 3 golds, all Norway) and
won 7 of 12 cross-country skiing golds. The other 5 cross-country golds? Sweden.
Together they shared all 12 events — no other country won a single cross-country
gold.

---

## Finding 2: The Netherlands Is Quietly Absurd

The Netherlands sent **39 athletes** and won **10 gold medals**.

Ten. Golds. From thirty-nine people.

That's 25.6 golds per 100 athletes — the best conversion rate of any nation.
Their secret was total domination of the ice oval: 5 golds in Short Track Speed
Skating, 5 more in Speed Skating. Jens van 't Wout alone won 3 golds.

The Netherlands is also famously flat. They just decided that going very fast in
circles on ice was their thing, and apparently committed.

---

## Finding 3: France Owned Biathlon

France won **6 of 11 biathlon gold events**. In a single discipline. At a single
Games.

Biathlon combines cross-country skiing with precision rifle shooting, which
sounds like it should be wide open. France disagreed. France's biathlon haul
alone — 6 golds — would rank them 8th on the overall gold table as a standalone
country.

Quentin Fillon Maillet and Julia Simon each won 4 medals (3 gold). The French
biathlon team walked away from Milano-Cortina looking like they run a very
specific finishing school.

---

## Finding 4: Brazil Won Alpine Skiing Gold

Lucas Pinheiro Braathen — a Brazilian alpine skier — won the Men's Giant Slalom.

Let that sink in. Brazil, which sits at 14°S latitude and has essentially no
alpine skiing infrastructure, produced the Olympic champion in one of winter
sports' most traditional events. It was the only Latin American gold medal of
the entire Winter Games.

From a pure data perspective: Brazil's $2.19 trillion economy produced 1 medal.
Norway's $483 billion economy produced 41. By the arithmetic of GDP-per-medal,
Norway's entire economy is smaller than the cost of a single Chinese medal
($18.3T / 15 = $1.22T each). But Brazil's single alpine skiing gold carries the
weight of an entire continental economy behind it — which makes it arguably the
most remarkable result of the Games.

---

## The Question: If You're Starting From Scratch, Which Sport Should You Choose?

We asked Claude to figure out which Olympic discipline gives an athlete the best
statistical odds of winning a medal — assuming they've already qualified for the
Games.

The answer required three separate analyses: **medal rate** (what fraction of
athletes in each sport actually medal), **country concentration** (does one
nation sweep everything), and **field size** (fewer competitors = better odds).

### The winner: Short Track Speed Skating

- **33.9% medal rate** — the highest of any discipline
- Gold spread across **4 countries** (genuinely competitive)
- Relay formats let one athlete stack medals across multiple events
- Only 112 athletes in the field

Short Track is the rare case where the headline number holds up: the medal rate
is high, the competition isn't monopolised by two countries, and the relay
structure multiplies your opportunities.

### The dark horse: Ski Mountaineering

It debuted at Milano-Cortina 2026. Only 36 athletes competed. The competitive
field is still forming. Get in early.

### What to avoid

**Nordic Combined**: Norway won all 3 gold events. Every single one. Unless
you're Norwegian, your gold odds are mathematically near zero.

**Alpine Skiing**: The most popular winter sport has the **worst medal rate** of
any discipline — just 9.2%. That's 306 athletes competing for 28 medals.
Expensive sport to train for, brutal to medal in.

**Ice Hockey**: 530 athletes, but gold goes to 23–25 players from a single
country per gender tournament. The pool looks big; the actual odds aren't.

---

## The GDP vs. Medals Question

One more analysis: does money buy winter Olympic medals?

The short answer is no. Or at least, not very efficiently.

Norway — whose entire GDP is roughly the size of the state of Washington —
produced 41 medals. China — the world's second-largest economy — produced 15.
Norway is **71x more efficient** per dollar of GDP than the United States, and
**103x more efficient** than China.

After controlling for latitude (northern countries have a natural advantage —
it's hard to build a cross-country skiing culture without snow), the real story
emerged. Georgia and Bulgaria, both sitting at roughly the same latitude as
Rome, **15x and 9x outperformed** their geographic peers. Latvia — population
1.8 million, GDP $43 billion — won 2 medals in bobsled and luge.

Great Britain, at the same latitude as Scandinavia, scored a latitude-adjusted
index of 0.11 — the lowest of any country in the dataset. Britain's sporting
culture has long pointed toward summer: athletics, rowing, cycling, tennis.
Every winter medal British athletes win is a genuine act of going against that
grain.

The conclusion across all three analyses: **latitude sets the ceiling; culture
determines how close you get to it.** You can't buy a ski jump in your DNA.

---

## Try It Yourself

The [full repository](https://github.com/kineticloom/plydb-fun-olympics-2026)
includes:

- All five Olympic datasets (athletes, coaches, medals, schedules, venues)
- GDP data from the World Bank
- Country latitude data
- A pre-configured `plydb-config.json` so you can start querying immediately
- Three full analysis writeups from Claude

To run it yourself:

1. Clone the repo
2. Follow the [Quickstart Guide](/docs/quickstart/) to install PlyDB and connect
   your AI agent
3. Tell your AI to use the `plydb-config.json` in the repo and start asking
   questions

The whole setup takes a few minutes. The rabbit holes take considerably longer.

---

Did you know? PlyDB can connect your AI to boring data too!

Whether it's business data in a dusty Excel sheet or a complex DevOps log in S3,
AI can be surprisingly good at making sense of a mess. PlyDB acts as the bridge,
letting your AI query across Postgres, MySQL, CSV, Excel, Parquet, Google
Sheets, and more - locally or in the cloud.

Open source and free.
[Give PlyDB a spin!](https://www.plydb.com/docs/quickstart/)
