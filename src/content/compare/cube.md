---
title: "PlyDB vs Cube: Direct Agent Gateway vs. Universal Semantic Layer"
description: "Cube is a universal semantic layer and agentic analytics platform — governed data models, pre-aggregation caching, REST/GraphQL/SQL APIs, and built-in AI agents. PlyDB gives AI agents direct SQL access to live sources, with semantic context built by agents themselves through OSI overlays that compound over time. Both are Apache 2.0 and ship MCP servers."
---

<div class="ply-compare">

<!-- ═══════════════════════════════════════════════════════════════
     HERO
     ═══════════════════════════════════════════════════════════════ -->
<section class="ply-cmp-hero">
  <div class="ply-inner">
    <div class="ply-cmp-hero__eyebrow">
      <span class="ply-cmp-hero__pill">Cube</span>
      <span class="ply-cmp-hero__sep">vs</span>
      <span class="ply-cmp-hero__pill ply-cmp-hero__pill--accent">PlyDB</span>
    </div>
    <h1>Cube vs <em>PlyDB</em></h1>
    <p class="ply-cmp-hero__sub">
      Cube (formerly Cube.js) is a universal semantic layer and agentic analytics platform — it sits between your data sources and every consumer of them, enforcing consistent metric definitions through engineer-authored data models, and caching results for sub-second performance. PlyDB takes a different approach: agents connect to live sources directly and build semantic context themselves through OSI overlays that accumulate across sessions. Both are Apache 2.0 open source and both ship native MCP servers. The core difference is the semantic model: engineer-defined and enforced upfront in Cube, or agent-built and compounding over time in PlyDB.
    </p>
  </div>
</section>

<hr class="ply-divider">

<!-- ═══════════════════════════════════════════════════════════════
     TL;DR TABLE
     ═══════════════════════════════════════════════════════════════ -->
<section class="ply-cmp-tldr">
  <div class="ply-inner">
    <span class="ply-section-label">At a glance</span>
    <h2 class="ply-section-title">TL;DR</h2>
    <div class="ply-cmp-table-wrap">
      <table class="ply-cmp-table">
        <thead>
          <tr>
            <th></th>
            <th>Cube</th>
            <th class="ply-cmp-th--ply">PlyDB</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>Semantic model approach</td>
            <td><span class="ply-cmp-val">Engineer-authored YAML/JS — measures, dimensions, and joins defined upfront and enforced consistently across all consumers</span></td>
            <td><span class="ply-cmp-val">Agent-authored OSI overlays — schema auto-discovered; agents accumulate business context across sessions that informs future queries</span></td>
          </tr>
          <tr>
            <td>Pre-aggregation &amp; query caching</td>
            <td><span class="ply-cmp-win">Built-in relational caching engine — automatically builds condensed datasets for sub-second analytics at scale</span></td>
            <td><span class="ply-cmp-val">No caching layer — queries execute live against source databases</span></td>
          </tr>
          <tr>
            <td>Multi-protocol APIs</td>
            <td><span class="ply-cmp-win">REST, GraphQL, SQL (Postgres wire protocol), MDX, and MCP — one semantic layer, any consumer</span></td>
            <td><span class="ply-cmp-val">MCP and CLI — purpose-built for AI agent access</span></td>
          </tr>
          <tr>
            <td>Built-in AI agents</td>
            <td><span class="ply-cmp-win">AI Data Analyst and AI Data Engineer agents — natural language analytics and automated model authoring via Cube Agentic Analytics</span></td>
            <td><span class="ply-cmp-val">Not applicable — PlyDB is the data layer; AI reasoning lives in your external agent</span></td>
          </tr>
          <tr>
            <td>Agent integration (MCP)</td>
            <td><span class="ply-cmp-val">Native MCP server — Premium and Enterprise plans only; exposes the semantic layer to MCP-compatible agents</span></td>
            <td><span class="ply-cmp-win">Native MCP &amp; CLI — available in the open-source binary, no paid plan required</span></td>
          </tr>
          <tr>
            <td>Time to first query</td>
            <td><span class="ply-cmp-val">Requires cube model authoring before agents or BI tools can query — hours to days depending on data complexity</span></td>
            <td><span class="ply-cmp-win">Minutes — auto-discovers schema; agents start querying immediately and build context over time</span></td>
          </tr>
          <tr>
            <td>Live operational DB access</td>
            <td><span class="ply-cmp-val">Queries go through the semantic model; pre-aggregations add a refresh cycle between source changes and query results</span></td>
            <td><span class="ply-cmp-win">Direct — agents query live data with no caching cycle in between</span></td>
          </tr>
          <tr>
            <td>Cross-source queries</td>
            <td><span class="ply-cmp-val">Supported via rollup joins across configured data sources — aggregated level, not arbitrary row-level JOINs</span></td>
            <td><span class="ply-cmp-win">Arbitrary SQL JOIN across any connected source in one query</span></td>
          </tr>
          <tr>
            <td>Semantic context for agents</td>
            <td><span class="ply-cmp-val">Engineer-defined measures and dimensions surface to agents via the API — consistent, governed, requires upfront authoring</span></td>
            <td><span class="ply-cmp-val">OSI overlays — agents auto-discover schema and write context that persists and compounds across sessions; advisory but substantive</span></td>
          </tr>
          <tr>
            <td>Deployment complexity</td>
            <td><span class="ply-cmp-val">Cube Core requires Kubernetes for production; Cube Cloud is fully managed SaaS</span></td>
            <td><span class="ply-cmp-win">Single binary, one JSON config file — runs anywhere without cluster infrastructure</span></td>
          </tr>
          <tr>
            <td>Open source</td>
            <td><span class="ply-cmp-val">Apache 2.0 (Cube Core); Cube Cloud is proprietary SaaS</span></td>
            <td><span class="ply-cmp-val">Apache 2.0</span></td>
          </tr>
          <tr>
            <td>Cost</td>
            <td><span class="ply-cmp-val">Cube Core: free; Cube Cloud: free tier (1K queries/day), CCU-based paid plans; MCP on Premium+</span></td>
            <td><span class="ply-cmp-win">Free and open source — no query limits, no paid tiers</span></td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</section>

<hr class="ply-divider">

<!-- ═══════════════════════════════════════════════════════════════
     DEFINITIONS
     ═══════════════════════════════════════════════════════════════ -->
<section class="ply-cmp-defs">
  <div class="ply-inner">
    <span class="ply-section-label">The tools</span>
    <h2 class="ply-section-title">What each one does</h2>
    <div class="ply-cmp-two-col">
      <div class="ply-cmp-def-card">
        <div class="ply-cmp-def-card__label">Cube</div>
        <h3>Universal Semantic Layer &amp; Agentic Analytics Platform</h3>
        <p>Cube is an open-source universal semantic layer that sits between your data sources and every consumer of them — BI tools, AI agents, embedded analytics, and custom applications. Data engineers define cubes, dimensions, measures, and joins in YAML or JavaScript; Cube enforces these definitions consistently across its REST, GraphQL, SQL (Postgres wire protocol), MDX, and MCP APIs. Its built-in relational caching engine materializes pre-aggregations for sub-second query performance at high concurrency. Cube Agentic Analytics (GA October 2025) adds AI Data Analyst and AI Data Engineer agents powered by Claude or a bring-your-own LLM. Cube Core is Apache 2.0; the MCP server requires a Cube Cloud Premium or Enterprise plan.</p>
      </div>
      <div class="ply-cmp-def-card ply-cmp-def-card--ply">
        <div class="ply-cmp-def-card__label">PlyDB</div>
        <h3>AI Agent Database Gateway</h3>
        <p>PlyDB is an open-source gateway built from the ground up for AI agents. You declare your data sources in a single JSON config file — PostgreSQL, MySQL, SQLite, S3, files, Google Sheets — and any AI agent connects immediately via native MCP or CLI, with no data modeling required before the first query. PlyDB's semantic context system auto-discovers schema and provides an OSI-format overlay system where agents record institutional knowledge — enum meanings, business rules, domain context — that persists and compounds across sessions. Read-only by design, single binary, and the MCP server ships with the open-source release at no additional cost.</p>
      </div>
    </div>
  </div>
</section>

<hr class="ply-divider">

<!-- ═══════════════════════════════════════════════════════════════
     WHEN TO CHOOSE
     ═══════════════════════════════════════════════════════════════ -->
<section class="ply-cmp-when">
  <div class="ply-inner">
    <span class="ply-section-label">The right fit</span>
    <h2 class="ply-section-title">When to use each</h2>
    <div class="ply-cmp-two-col">
      <div class="ply-cmp-when-card">
        <h3>Choose Cube when&hellip;</h3>
        <ul class="ply-cmp-when-list">
          <li>Metric consistency is non-negotiable — "Revenue" must mean the same thing whether a BI tool, an AI agent, or an embedded app is asking</li>
          <li>Query performance at scale matters — pre-aggregations serve sub-second responses at high concurrency without hitting the warehouse on every request</li>
          <li>You need to serve multiple consumer types from one semantic model — REST, GraphQL, SQL, MCP, and embedded analytics all from a single definition</li>
          <li>You have data engineering resources to author and maintain the cube model — and want AI (Cube Copilot) to assist with that authoring</li>
          <li>You want built-in AI agents for natural language analytics on top of a governed semantic layer</li>
        </ul>
      </div>
      <div class="ply-cmp-when-card ply-cmp-when-card--ply">
        <h3>Choose PlyDB when&hellip;</h3>
        <ul class="ply-cmp-when-list">
          <li>You want agents querying live data immediately — no cube model to author, no pre-aggregation refresh cycle between source changes and query results</li>
          <li>You want semantic context that builds from real agent sessions — OSI overlays accumulate institutional knowledge across conversations without requiring a data engineer to model it first</li>
          <li>Arbitrary SQL JOINs across multiple sources are required — not just aggregated rollup joins between cubes</li>
          <li>MCP should be available without a paid plan — Cube's MCP server requires Premium or Enterprise</li>
          <li>You want a single binary with no cluster infrastructure — Cube Core production deployments require Kubernetes</li>
        </ul>
      </div>
    </div>
    <p style="margin-top: 2rem; font-size: 0.9rem; color: var(--ply-text-dim); line-height: 1.65; max-width: 700px;">
      <strong style="color: var(--ply-text);">These tools can complement each other.</strong> Teams running Cube for governed BI and multi-consumer analytics can deploy PlyDB alongside it as the agent gateway to operational databases and ad-hoc sources that don't belong in a cube model — sources that still need to be reachable by agents without going through a modeling phase first.
    </p>
  </div>
</section>

<hr class="ply-divider">

<!-- ═══════════════════════════════════════════════════════════════
     FOOTER CTA
     ═══════════════════════════════════════════════════════════════ -->
<section class="ply-footer-cta">
  <div class="ply-inner">
    <h2>Semantic context that grows with your agents.</h2>
    <p>Auto-discover schema. OSI overlays that compound across sessions. MCP included. Free and open source.</p>
    <div class="ply-cta-row">
      <a href="/docs/quickstart/" class="ply-btn ply-btn--primary">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M4 19.5A2.5 2.5 0 016.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 014 19.5v-15A2.5 2.5 0 016.5 2z"/></svg>
        Get Started
      </a>
      <a href="https://github.com/kineticloom/plydb" class="ply-btn ply-btn--ghost" target="_blank" rel="noopener">
        <svg viewBox="0 0 24 24" fill="currentColor"><path d="M12 0C5.37 0 0 5.37 0 12c0 5.31 3.435 9.795 8.205 11.385.6.105.825-.255.825-.57 0-.285-.015-1.23-.015-2.235-3.015.555-3.795-.735-4.035-1.41-.135-.345-.72-1.41-1.23-1.695-.42-.225-1.02-.78-.015-.795.945-.015 1.62.87 1.845 1.23 1.08 1.815 2.805 1.305 3.495.99.105-.78.42-1.305.765-1.605-2.67-.3-5.46-1.335-5.46-5.925 0-1.305.465-2.385 1.23-3.225-.12-.3-.54-1.53.12-3.18 0 0 1.005-.315 3.3 1.23.96-.27 1.98-.405 3-.405s2.04.135 3 .405c2.295-1.56 3.3-1.23 3.3-1.23.66 1.65.24 2.88.12 3.18.765.84 1.23 1.905 1.23 3.225 0 4.605-2.805 5.625-5.475 5.925.435.375.81 1.095.81 2.22 0 1.605-.015 2.895-.015 3.3 0 .315.225.69.825.57A12.02 12.02 0 0024 12c0-6.63-5.37-12-12-12z"/></svg>
        GitHub
      </a>
    </div>
    <p class="ply-license">Apache License 2.0</p>
  </div>
</section>

</div>
