---
title: "PlyDB vs dbt: Live Agent Query Gateway vs. SQL Transformation Framework"
description: "dbt transforms raw data into trusted models inside your warehouse — CI/CD, testing, lineage, and a governed semantic layer. PlyDB gives AI agents live SQL access to databases that haven't been loaded into a warehouse yet. They solve different problems and often run together."
---

<div class="ply-compare">

<!-- ═══════════════════════════════════════════════════════════════
     HERO
     ═══════════════════════════════════════════════════════════════ -->
<section class="ply-cmp-hero">
  <div class="ply-inner">
    <div class="ply-cmp-hero__eyebrow">
      <span class="ply-cmp-hero__pill">dbt</span>
      <span class="ply-cmp-hero__sep">vs</span>
      <span class="ply-cmp-hero__pill ply-cmp-hero__pill--accent">PlyDB</span>
    </div>
    <h1>dbt vs <em>PlyDB</em></h1>
    <p class="ply-cmp-hero__sub">
      dbt and PlyDB operate at different points in the data stack. dbt is a SQL transformation framework — it takes raw data already loaded into your warehouse and builds governed, tested, documented models inside it. PlyDB is a query gateway — it gives AI agents live SQL access to operational databases, files, and SaaS sources that haven't been loaded anywhere. Both ship native MCP servers, but they expose fundamentally different things.
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
            <th>dbt</th>
            <th class="ply-cmp-th--ply">PlyDB</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>SQL transformations inside a warehouse</td>
            <td><span class="ply-cmp-win">Core purpose — builds trusted, versioned data models via SQL in your warehouse</span></td>
            <td><span class="ply-cmp-val">Not applicable — PlyDB is a read-only query gateway, not a transformation tool</span></td>
          </tr>
          <tr>
            <td>Data testing &amp; quality enforcement</td>
            <td><span class="ply-cmp-win">Built-in — assert column uniqueness, non-null, referential integrity; custom tests in YAML</span></td>
            <td><span class="ply-cmp-val">Not applicable</span></td>
          </tr>
          <tr>
            <td>Data lineage &amp; documentation</td>
            <td><span class="ply-cmp-win">Auto-generated lineage DAGs and documentation from model definitions</span></td>
            <td><span class="ply-cmp-val">Not applicable</span></td>
          </tr>
          <tr>
            <td>Governed semantic layer (MetricFlow)</td>
            <td><span class="ply-cmp-win">MetricFlow (Apache 2.0) — define metrics once, enforce everywhere, query via any tool or agent</span></td>
            <td><span class="ply-cmp-val">Agent-authored OSI overlays — discovered and written by agents, compounding over time</span></td>
          </tr>
          <tr>
            <td>CI/CD for data models</td>
            <td><span class="ply-cmp-win">Native — slim CI, PR checks, job scheduling in dbt Cloud; dbt Core works in any pipeline</span></td>
            <td><span class="ply-cmp-val">Not applicable</span></td>
          </tr>
          <tr>
            <td>Agent integration (MCP)</td>
            <td><span class="ply-cmp-val">Native MCP server — exposes dbt metadata, compiler, MetricFlow engine, and lineage to agents</span></td>
            <td><span class="ply-cmp-val">Native MCP &amp; CLI — exposes live database queries to agents</span></td>
          </tr>
          <tr>
            <td>Live access to operational DBs (no ingestion)</td>
            <td><span class="ply-cmp-val">No — dbt transforms data already in a warehouse; it does not query live operational databases</span></td>
            <td><span class="ply-cmp-win">Direct — queries live sources with no prior loading or pipeline required</span></td>
          </tr>
          <tr>
            <td>Cross-source queries at query time</td>
            <td><span class="ply-cmp-val">No native federation — data must be loaded into the same warehouse before cross-source joins</span></td>
            <td><span class="ply-cmp-win">JOIN across any connected source in one query</span></td>
          </tr>
          <tr>
            <td>Semantic context for agents</td>
            <td><span class="ply-cmp-val">MetricFlow metrics and dbt model docs — requires models to be authored and deployed first</span></td>
            <td><span class="ply-cmp-win">Auto-discovery + OSI overlays that compound across sessions with no upfront authoring</span></td>
          </tr>
          <tr>
            <td>Time to first query on live data</td>
            <td><span class="ply-cmp-val">Requires a warehouse, source loads, and model authoring before agents can query results</span></td>
            <td><span class="ply-cmp-win">Minutes — single binary, one JSON config file, no pipeline required</span></td>
          </tr>
          <tr>
            <td>Open source</td>
            <td><span class="ply-cmp-val">dbt Core: Apache 2.0; MetricFlow: Apache 2.0; dbt Cloud: proprietary SaaS</span></td>
            <td><span class="ply-cmp-val">Apache 2.0</span></td>
          </tr>
          <tr>
            <td>Cost</td>
            <td><span class="ply-cmp-val">dbt Core: free; dbt Cloud: free Developer tier, ~$100/developer/month (Starter)</span></td>
            <td><span class="ply-cmp-win">Free and open source</span></td>
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
        <div class="ply-cmp-def-card__label">dbt</div>
        <h3>SQL Transformation Framework</h3>
        <p>dbt (data build tool) is the standard tool for the "T" in ELT — transforming raw data that has already been loaded into a warehouse into trusted, documented, tested data models. You write SQL SELECT statements; dbt handles materialization, dependency ordering, testing, and documentation. dbt Core is Apache 2.0 and runs anywhere; dbt Cloud adds scheduling, a browser IDE, CI/CD integrations, and the dbt Copilot AI assistant. The dbt Semantic Layer, powered by MetricFlow (open-sourced in 2025 under Apache 2.0), defines business metrics in code so they can be queried consistently by BI tools, AI agents, and APIs. A native MCP server exposes dbt metadata, the compiler, and MetricFlow to AI agents — enabling them to generate SQL, run models, and query governed metrics — but the underlying data still needs to be in a warehouse first.</p>
      </div>
      <div class="ply-cmp-def-card ply-cmp-def-card--ply">
        <div class="ply-cmp-def-card__label">PlyDB</div>
        <h3>AI Agent Database Gateway</h3>
        <p>PlyDB is an open-source gateway built from the ground up for AI agents. You declare your data sources in a single JSON config file — PostgreSQL, MySQL, SQLite, S3, files, Google Sheets — and any AI agent connects immediately via native MCP or CLI, with no data ingestion or transformation pipeline required. PlyDB's semantic context system takes a different approach from MetricFlow: it auto-discovers schema and provides an OSI-format overlay system where agents themselves record institutional knowledge — enum meanings, business rules, domain context learned in conversation — that persists and compounds across sessions. The goal is direct, live access to data wherever it already lives, with no upfront modeling phase.</p>
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
        <h3>Use dbt when&hellip;</h3>
        <ul class="ply-cmp-when-list">
          <li>You need to transform raw warehouse data into clean, tested, documented models that analysts and agents can trust</li>
          <li>Metric consistency matters — define "Revenue" or "Active Users" once in MetricFlow and enforce it everywhere</li>
          <li>Data quality is a requirement — column-level tests, referential integrity checks, and CI runs on every PR</li>
          <li>You want lineage, documentation, and version control for your data models as code</li>
          <li>Your warehouse is the source of truth and you want governed, reproducible transformations running on a schedule</li>
        </ul>
      </div>
      <div class="ply-cmp-when-card ply-cmp-when-card--ply">
        <h3>Use PlyDB when&hellip;</h3>
        <ul class="ply-cmp-when-list">
          <li>Your AI agent needs to query live operational databases right now — not data that's been loaded and transformed first</li>
          <li>The data doesn't belong in a warehouse and never will — production PostgreSQL, MySQL, local files, or SaaS exports</li>
          <li>You need cross-source JOINs at query time against sources that dbt can't federate across</li>
          <li>Semantic context should grow from agent sessions, not require a modeling phase before agents can begin</li>
          <li>You want zero infrastructure overhead — no warehouse, no pipeline, no scheduler</li>
        </ul>
      </div>
    </div>
    <p style="margin-top: 2rem; font-size: 0.9rem; color: var(--ply-text-dim); line-height: 1.65; max-width: 700px;">
      <strong style="color: var(--ply-text);">These tools are designed to coexist.</strong> dbt builds and governs the modeled layer in your warehouse; PlyDB gives agents live access to the operational layer that sits outside it. A team running dbt on Snowflake for analytics can deploy PlyDB alongside it to give agents direct access to production PostgreSQL, raw S3 exports, or SaaS data that feeds the pipeline — without routing everything through dbt first.
    </p>
  </div>
</section>

<hr class="ply-divider">

<!-- ═══════════════════════════════════════════════════════════════
     FOOTER CTA
     ═══════════════════════════════════════════════════════════════ -->
<section class="ply-footer-cta">
  <div class="ply-inner">
    <h2>Live data access for your agents. No pipeline required.</h2>
    <p>Query operational databases directly. Native MCP. Semantic context that compounds. Free and open source.</p>
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
