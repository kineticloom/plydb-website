---
date: "2026-03-28"
draft: false
title: "AI-Powered Google Analytics 4 Analysis — No SQL, No Data Warehouse"
description:
  "Query your Google Analytics 4 data in plain English with an AI agent — no
  SQL, no warehouse. Combine GA4 with Stripe, ad spend, CRM, and more in a
  single query."
---

Today we're releasing the
**[Google Analytics AI Starter](https://github.com/kineticloom/plydb-example-google-analytics)**:
an open-source project that downloads your Google Analytics 4 data locally and
gives your AI agent SQL access via [PlyDB](https://www.plydb.com/) — so you can
ask questions about traffic, content performance, and acquisition in plain
English.

Google now offers a
[first-party GA4 MCP server](https://developers.google.com/analytics/devguides/MCP)
that connects AI agents directly to your analytics account — a reasonable option
if live API access is what you need. This project takes a different approach:
your GA4 data downloads as local Parquet files, queried by PlyDB alongside any
other data source you connect.

Two things make that worth considering. First, it's entirely local — no running
servers, no cloud infrastructure, no data leaving your machine after the initial
download. Second, because PlyDB queries across multiple sources in a single SQL
statement, GA4 is just the starting point: connect a Stripe export, a
PostgreSQL database, files in S3, a Google Sheet, or any other
PlyDB-supported source and ask questions that span all of them at once —
something a single-source MCP connection can't do on its own.

---

## How AI-Powered Google Analytics Analysis Works

**The Google Analytics AI Starter is an open-source project that connects your
AI agent to your Google Analytics 4 data so you can ask questions about
traffic, content performance, and acquisition in plain English — no SQL, no
data warehouse, no BI tool required.**

{{< ga4-analytics-arch >}}

Here's how it works:

1. **The Google Analytics Data API** pulls your GA4 data — sessions, users, page
   views, events, traffic sources, and geographic segments — and saves it as
   local Parquet files, partitioned by date. Your data never leaves your machine.
2. **[PlyDB](https://www.plydb.com/)** — the open-source universal database
   gateway for AI agents — sits between your agent and your data sources,
   translating plain-English questions into SQL and executing them.
3. A **semantic overlay** bundled with the repo teaches the agent GA4's data
   model: how reports are structured, which dimensions map to which metrics, and
   what each field means in practice.
4. You ask questions in plain English. The agent translates them into SQL,
   analyzes the results, and returns answers — not just numbers, but patterns,
   diagnoses, and recommendations. No SQL required. Works with Claude, ChatGPT,
   Gemini, and any MCP- or CLI-compatible agent.

Your data stays local. GA4 data is as fresh as your last download — run the
script on a schedule to keep it current.

**Who is this for?** Growth teams, product managers, and marketing analysts who
want to do real GA4 analysis without standing up a data warehouse — and
developers who want a working example of AI-powered web analytics to adapt for
their own stack.

---

## What You Can Ask Your Google Analytics Data

The value isn't in asking "how many sessions did we get last month" — GA4 shows
you that already. It's in prompts that produce decisions, not just data: where
the agent applies multi-step reasoning, cross-references signals, and returns a
recommendation rather than a table.

**Content portfolio audit.** Ask the agent to classify every page by the gap
between traffic and engagement — high traffic with high bounce rates signals
intent mismatch; strong engagement with low traffic signals a distribution gap;
low on both means a candidate for retirement. Ask it to produce a prioritized
action list: pages to redesign, pages to promote harder, and pages to cut. This
is the kind of multi-step analysis that takes an analyst a full day; the agent
does it in one conversation.

**Campaign performance verdict.** Ask the agent to evaluate your latest paid or
social campaign: compare its traffic against your organic baseline on bounce rate
and average session duration, identify which landing pages it's hitting and how
they're performing, and render a verdict — scale it, redirect it to a
better-performing page, or cut it. The agent applies consistent scoring across
every source/medium combination and returns a ranked recommendation, not a chart
to interpret yourself.

**Acquisition channel strategy.** Ask the agent to score every source/medium
combination by engagement quality against session volume, identify the gap
between the two, and flag where to reallocate: channels that are all volume with
no engagement, channels that are high quality but underinvested, and the ones
worth doubling down on before your next budget review. One prompt; a channel
allocation case ready to take into a planning meeting.

**Weekly briefing with anomaly diagnosis.** Ask the agent what changed most in
the last 7 days compared to the prior 7 days — across sessions, bounce rate, and
top pages — and to hypothesize why each significant change happened. You get
changes ranked by magnitude, possible explanations attached, and a suggested
next step for each one worth acting on. Not a dashboard to open — a briefing to
act on.

**Distribution channel scouting.** Ask the agent to find referral sources
sending small but highly engaged traffic that you're not actively cultivating —
ranked by quality-to-volume ratio. For the top performers, the agent flags them
as underdeveloped distribution opportunities and explains what makes their
traffic different. The kind of signal that compounds into a real acquisition
channel before competitors notice it.

**Growth market identification.** Ask the agent which international markets show
a rising session trend and a high returning-visitor rate but have no localized
content or active campaign investment yet. It cross-references session volume,
growth trajectory, and new-vs-returning visitor ratio to rank markets by
investment priority — not a table of countries, but a ranked case for where to
expand next and why.

---

## Going Further

GA4 is a starting point. PlyDB queries local files and remote sources alike, so
the same agent setup scales naturally to richer data, automated workflows, and
a semantic layer that deepens over time.

---

### Cross-Source Attribution

Traffic data in isolation answers "where do visitors come from." Traffic data
joined with revenue answers "which sources produce customers worth keeping."
PlyDB queries across multiple data sources in a single SQL statement — so you
can join GA4 acquisition data with revenue from
[Stripe](../plydb-example-stripe), ad spend exports
from Google Ads or Facebook, or CRM data, without moving anything to a
warehouse.

{{< ga4-analytics-extend-multisource >}}

---

### Proactive Monitoring Instead of Static Dashboards

A GA4 dashboard tells you what the numbers are. It doesn't tell you what
changed, why it changed, or what to do about it. The alternative is to schedule
your agent to run nightly against fresh data and send a briefing: a traffic drop
on an organic page that was working last week, a bounce rate spike on your
highest-converting landing page, a referral source whose quality score quietly
doubled. Anomalies are flagged and context is attached. You get a briefing
instead of a chart.

{{< ga4-analytics-extend-monitoring >}}

---

### From Insight to Action

Proactive monitoring surfaces what changed. The more powerful step is closing
the loop in the same session: the agent identifies the problem and drafts the
response.

Ask the agent to run a content audit, identify the pages with the biggest
traffic-to-engagement gap, and produce a brief for each — what the page is
currently doing, why it's likely failing (intent mismatch, thin content, missing
CTA), and a suggested structure for a rewrite. Instead of switching to a
separate document, you get the analysis and the starting point for the fix in
one conversation.

The same applies to campaigns: the agent detects underperformance, explains the
likely cause, and drafts a revised targeting or landing page recommendation
ranked by expected impact. You review, adjust, and move.

{{< ga4-analytics-extend-action >}}

---

### Richer Semantic Context Over Time

The `plydb-overlay.yaml` file bundled with the repo teaches the agent GA4's data
model from day one. But the real value compounds. After any analysis session, ask
your agent to update the overlay with what it learned — your site's key pages,
event naming conventions, campaign taxonomy, which sources are known noise.
Future sessions inherit that context automatically, so the agent's first query in
a new conversation is already informed by everything it has learned before.

---

## Get Started with the Google Analytics AI Starter

The [Google Analytics AI Starter](https://github.com/kineticloom/plydb-example-google-analytics) has
everything you need: a download script for the Google Analytics Data API, a
pre-configured PlyDB setup, and a semantic overlay so your agent understands
GA4's data model out of the box.

You'll need a Google Cloud project (free tier works), Python 3.8+, and PlyDB
installed. The README walks through the rest in four steps.

---

## Frequently Asked Questions

**Do I need to know SQL to use this?** No. Your AI agent handles all the SQL.
You ask questions in plain English; the agent translates them into queries, runs
them via PlyDB, and returns the answer.

**What's the difference between this and the GA4 Explorations tool?** GA4
Explorations lets you build custom reports within GA4's interface, but you're
limited to its report structure and can't apply arbitrary business logic or
cross-reference external data. This gives you full SQL access to your raw GA4
data — you can ask any question, apply any logic, and join with any other data
source PlyDB supports.

**How do I analyze Google Analytics traffic trends with AI?** Download your GA4
data with the script (one command), then open Claude Code or any
PlyDB-compatible agent in the project directory and ask: _"How has organic
search traffic trended week-over-week over the past year?"_ The agent queries
the local Parquet files via PlyDB and returns the answer.

**Does my GA4 data leave my machine?** No. The Google Analytics Data API
downloads your data to local Parquet files. PlyDB is read-only by default — your
AI can analyze and find patterns, but it cannot alter your data. Nothing is sent
to any external service beyond the initial API call to download your data.

**Is the data real-time?** Data is as fresh as your last download run. Run the
download script on a schedule (cron, GitHub Actions, Airflow) to keep it current
— PlyDB queries the updated files without any pipeline changes.

**What GA4 data does it cover?** Four reports: traffic sources (sessions, users,
bounce rate, avg session duration by source/medium/campaign/landing page), page
views (sessions and engagement by page path/title), events (event counts and
users by event name), and user segments (sessions and users by country, device,
browser, and OS).

**Do I need Google Analytics 360 or a paid tier?** No. The Google Analytics Data
API is available on the free tier of GA4. You do need a Google Cloud project to
create API credentials, but the project itself can remain on the free tier.

**Which AI agents does this work with?** Any agent that supports MCP or CLI
tools — including Claude Code, Claude Desktop, ChatGPT, Gemini, Codex, and more.
See the [PlyDB documentation](https://www.plydb.com/docs/) for the full
compatibility list.

**Can I add other data sources alongside GA4?** Yes — that's one of the most
natural extensions. PlyDB supports cross-source SQL queries, so you can add
Stripe revenue data, ad spend exports, or any other supported source and query
across all of them in a single statement.

**Is this production-ready?** It's a starter project. It works, and the semantic
overlay is thorough enough for serious analysis. For production use you'd likely
want to schedule the data loads and extend the overlay with your own site's
context — key pages, event naming conventions, campaign taxonomy. The repo README
covers the main adaptation paths.

---

The GA4 interface is great at showing you what happened. This is for when you
need to understand why, and what to do next.

[Try the Google Analytics AI Starter →](https://github.com/kineticloom/plydb-example-google-analytics)

---

Already building something similar with PlyDB? We'd love to hear about it. PlyDB
is open source — [join us on GitHub](https://github.com/kineticloom/plydb).
