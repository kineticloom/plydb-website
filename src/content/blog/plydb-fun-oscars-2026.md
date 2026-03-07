---
date: "2026-03-07"
draft: false
title:
  "Fun with PlyDB: We Fed an AI 97 Years of Oscar Data and Asked It to Pick the Winners"
description:
  "We gave Claude 97 years of Oscar history, every major precursor award, and a SQL interface. Then we asked it to pick the 2026 winners. Here's what it said."
---

The 98th Academy Awards are in eight days. We gave Claude a database of every
Oscar nomination since the 1st ceremony, precursor award data going back
decades, and [PlyDB](https://www.plydb.com/) to query it all. Then we asked it
to pick the winners.

No gut feelings. No box office. No Hollywood intuition. Just an AI, a
probability model, and 97 years of data.

Here are its predictions:

{{< oscars-2026-predictions >}}

The two sharpest calls: **Jessie Buckley at 90%** and **Paul Thomas Anderson at
89%**. The most honest shrug: **Best Actor**, where the model couldn't separate
the top three nominees by more than 5 percentage points.

---

## The Setup

PlyDB is an open-source tool that lets AI agents query your data directly — no
pipelines, no data wrangling, no ETL. You point it at your files, and the AI
can ask SQL questions against them in real time. An AI grounded in real data
gives you answers you can actually trace — not confident guesses built on
training-set impressions.

We connected it to historical Oscar nomination data covering all 97 past
ceremonies, plus precursor award datasets: BAFTA, Golden Globes, SAG Awards,
Critics Choice, and TIFF People's Choice. Then Claude asked itself a question
in plain English: *historically, when a film or performer wins one of these
precursor awards, how often do they go on to win the Oscar?*

The rest follows from that question.

---

## Which Awards Actually Predict Oscars?

Not all precursor awards are created equal. Before scoring the 2026 nominees,
the model needed to establish conversion rates from historical data — how often
does each precursor translate to an Oscar win?

{{< oscars-2026-precursors >}}

A few things jump out immediately:

**SAG Awards are the strongest predictor for acting categories** — conversion
rates between 67% and 77% across all four acting categories. If SAG voters
like you, Academy voters usually agree.

**Critics Choice leads for Best Picture and Best Director** — 60% and 71.9%
respectively. These are strong numbers.

**BAFTA is the weakest predictor in every single category**, despite its
prestige. BAFTA's voter base skews toward British and international cinema in
ways that diverge reliably from Academy tastes. A BAFTA win is a signal, but
the weakest one available.

**The Golden Globe Comedy/Musical is nearly worthless for Oscar prediction.**
The GG Best Actor in a Comedy or Musical has converted to an Oscar win only
9.2% of the time across 76 years. It's a beautiful award. It means almost
nothing for March 15th. This matters a lot for Best Actor — more on that below.

---

## The Model

With conversion rates in hand, the model works in two steps.

**Step 1 — Signal score.** Each nominee accumulates points equal to the
historical Oscar conversion rate of each precursor they won. Win the SAG Lead
Actor? +77.4 points. Win the Critics Choice? +64.5 points. Win nothing? Zero.

**Step 2 — Upset adjustment.** Occasionally the Oscar goes to someone who won
none of the major precursors. How often? We queried it:

| Category | Historical upset rate |
|---|---|
| Best Actress | 0.0% |
| Best Supporting Actress | 3.3% |
| Best Actor | 6.7% |
| Best Supporting Actor | 6.7% |
| Best Picture | 13.3% |
| Best Director | 13.3% |

The formula distributes probability in two parts: a large share proportional to
each nominee's precursor signal, plus a flat floor for every nominee that
accounts for the upset rate.

$$P(i) = \frac{\text{signal}(i)}{\sum \text{signals}} \times (1 - \text{upset\_rate}) + \frac{\text{upset\_rate}}{N}$$

In plain English: most of the probability mass goes to whoever has the
strongest precursor signal. A small reserve is split equally across all
nominees — because the upset does happen, and we can't predict which zero-signal
nominee gets lucky.

---

## Category by Category

### Best Actress: The Most Lopsided Race (90%)

{{< oscars-2026-category id="actress" >}}

**Predicted winner: Jessie Buckley** (*Hamnet*)

Buckley won 4 of 5 major precursors: BAFTA, GG Drama, Critics Choice, and SAG.
The closest recent parallel that didn't convert is Cate Blanchett for *Tár*
(2022) — she swept BAFTA, GG Drama, and Critics Choice but lost the SAG to
Michelle Yeoh, who then went on to win the Oscar. Buckley won all four.

Renée Zellweger (*Judy*, 2019), Frances McDormand (*Three Billboards*, 2017),
and Brie Larson (*Room*, 2015) all swept BAFTA + GG Drama + Critics Choice +
SAG, and all three won the Oscar. The historical upset rate for Best Actress is
**0.0%** — the Oscar has never gone to a nominee who won zero of the tracked
precursors.

Rose Byrne (*If I Had Legs I'd Kick You*) won the GG Comedy, which carries a
22.8% conversion rate for Best Actress — giving her the remaining 10%. It's the
only other signal in this category.

### Best Director: A Clean Sweep (89%)

{{< oscars-2026-category id="director" >}}

**Predicted winner: Paul Thomas Anderson** (*One Battle after Another*)

PTA won all three available directing precursors — BAFTA, Golden Globe, Critics
Choice. Every other nominee won zero. No other directing race in recent memory
has been this concentrated.

The remaining 11% is almost entirely structural: the 13.3% historical upset
rate, spread across 5 nominees, forces a 2.7% floor for each non-PTA nominee
regardless of their zero signal. The most credible upset candidate is Ryan
Coogler (*Sinners*), whose film leads the entire field with 16 Oscar
nominations — but without a single directing precursor, the data has nothing
to work with.

There's also a structural tailwind here: since 1980, Best Picture and Best
Director have gone to the same film **73.3% of the time**. PTA's near-lock in
directing is a meaningful correlated signal for *One Battle after Another* in
Best Picture too.

### Best Actor: Anyone's Guess (35/33/30%)

{{< oscars-2026-category id="actor" >}}

**Predicted winner: Michael B. Jordan** (*Sinners*) — barely

This is the most honest toss-up the model produced. Jordan won the SAG (77.4%
predictor — the strongest single acting signal available). Chalamet won GG
Comedy and Critics Choice. Moura won GG Drama. Probabilities: Jordan 35%,
Chalamet 33%, Moura 30%.

Here's where the GG Comedy caveat matters enormously. Chalamet's Golden Globe
win carries only 9.2% historical weight. Strip that out and his effective signal
is just Critics Choice (64.5%) — essentially equal to Moura's GG Drama (65.8%).
The race is genuinely three-way, and the differences between the candidates are
smaller than the model's noise.

BAFTA is also an outlier this year: their Best Actor winner was Robert Aramayo
(*I Swear*), a film that received no Oscar nominations at all. When the BAFTA
acting pick doesn't even make the Oscar field, BAFTA's signal contribution
disappears — which it did here.

### Best Picture: Three Films in the Conversation (45%)

{{< oscars-2026-category id="picture" >}}

**Predicted winner: One Battle after Another**

Seven of the ten Best Picture nominees won zero precursor awards. Three are in
an actual race.

*One Battle after Another* holds the highest-value signals: Critics Choice
(60%), BAFTA (31.6%), and GG Comedy (16.7%). The Golden Globe classified it as
Comedy/Musical rather than Drama — which is why *Hamnet* picked up GG Drama
while *One Battle* got the lesser GG Comedy signal. But Critics Choice and
BAFTA carry enough weight to put *One Battle* clearly in front at 45%.

*Hamnet* sits at 26%, driven mainly by GG Drama (46.7%) plus TIFF (14.9%). TIFF
People's Choice has only matched the Oscar Best Picture winner 7 times in 47
years — it's included in the model but carries little weight.

*Sinners* at 19% rests entirely on its SAG Cast win (43.3% historical
conversion). But there's a qualitative case the signal model can't fully
capture: **16 Oscar nominations across 16 different departments** — one
nomination in essentially every eligible category, an all-time record. The
model knows about the SAG Cast win. It doesn't know how to value the Academy's
collective respect for every craftsperson on a single film. That residual is
real, and it's why 19% probably undersells *Sinners*.

### Best Supporting Actor: Penn vs. Elordi (59/37%)

{{< oscars-2026-category id="supporting-actor" >}}

**Predicted winner: Sean Penn** (*One Battle after Another*)

Penn won both BAFTA and SAG, which gives him a clear signal lead. Jacob
Elordi's Critics Choice win (61.3%) keeps the race genuinely competitive at
37%. One structural wrinkle the model can't resolve: both Penn and Benicio Del
Toro are nominated from the same film. Vote-splitting between two nominees from
*One Battle after Another* could benefit Elordi at the margins — but historical
data can't reliably quantify that effect.

The historical upset rate here is just 6.7%, covering two instances in 30
years: James Coburn (1998) and George Clooney (2005).

### Best Supporting Actress: Strong Data Pick (76%)

{{< oscars-2026-category id="supporting-actress" >}}

**Predicted winner: Amy Madigan** (*Weapons*)

Madigan won both SAG and Critics Choice — the two highest-weight sources in
this category. The model's one unresolved tension: *Weapons* received no other
Oscar nominations in 2026. The sole historical upset in this category was Marcia
Gay Harden for *Pollock* in 2000 — a winner from a film with minimal broader
Academy support. That's exactly the *Weapons* situation.

Wunmi Mosaku (*Sinners*) sits at 22% on her BAFTA win alone, with the
16-nomination breadth of *Sinners* adding qualitative weight the model can't
capture. She's the most credible upset candidate.

---

## Numbers Worth Noting

**Sinners' 16-department sweep** is genuinely unprecedented — one nomination in
virtually every eligible category at a single ceremony. The most-nominated film
rarely wins Best Picture (see: *The Power of the Dog*, 2022; *Mank*, 2021;
*The Irishman*, 2019). But the breadth of Academy support it represents is
something the signal model only partially reflects.

**The GG Comedy trap, quantified.** The Golden Globe Best Actor in a Comedy or
Musical has converted to an Oscar win 7 times in 76 years (9.2%). Timothée
Chalamet's win is the most misleadingly prestigious precursor in the 2026 field.

**BAFTA's acting divergence.** Two BAFTA acting nominees are completely absent
from the Oscar race: Robert Aramayo (Lead Actor, *I Swear*) and Chase Infiniti
(Lead Actress nominee, *One Battle after Another*). When BAFTA's acting picks
diverge this sharply from the Academy's, the BAFTA acting wins carry even less
predictive weight than their already-low historical baseline.

**The Best Picture/Best Director alignment.** Since 1980, both Oscars went to
the same film 73.3% of the time. Notable exceptions: *CODA* won Best Picture
while Jane Campion won Best Director for *The Power of the Dog* (2022); *Green
Book* won Best Picture while Alfonso Cuarón won Best Director for *Roma* (2019).
PTA's near-lock in directing is a structurally correlated signal — even though
the model treats the two categories independently.

**The only historical Best Actress upset rate: 0.0%.** In every year the model
tracked, the Best Actress Oscar went to someone who had won at least one of the
major precursors. If the model has one near-certainty on the night, it's that
Jessie Buckley wins.

---

## Try It Yourself

The full repository — all datasets, parsing and normalization scripts, and
analysis — is at
[github.com/kineticloom/plydb-fun-oscars-2026](https://github.com/kineticloom/plydb-fun-oscars-2026).

It includes historical Oscar nominations back to the 1st ceremony, plus
precursor award data for BAFTA, Golden Globes, SAG, Critics Choice, and TIFF.
Everything is pre-normalized and ready to query against, with a
`plydb-config.json` so you can start chatting with the data immediately.

To run your own queries:

1. Clone the repo
2. Follow the [Quickstart Guide](/docs/quickstart/) to install PlyDB and connect
   your AI agent
3. Point your AI at the `plydb-config.json` and start asking questions

We'll be back on March 16th with how many we got right.

---

Did you know? PlyDB can connect your AI to boring data too!

Whether it's business data in a dusty Excel sheet or a complex DevOps log in
S3, AI can be surprisingly good at making sense of a mess. PlyDB acts as the
bridge, letting your AI query across Postgres, MySQL, CSV, Excel, Parquet,
Google Sheets, and more — locally or in the cloud.

Open source and free.
[Give PlyDB a spin!](https://www.plydb.com/docs/quickstart/)
