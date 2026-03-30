---
title: "PlyDB vs Wren AI: Direct SQL Agent Gateway vs. Text-to-SQL Semantic Platform"
description: "Wren AI is an open-source GenBI agent that translates natural language into SQL through a governed semantic layer (MDL). PlyDB gives AI agents direct SQL access to live databases with no translation layer in the way. Both are read-only, both ship MCP servers — the difference is where SQL reasoning lives."
---

<div class="ply-compare">

<!-- ═══════════════════════════════════════════════════════════════
     HERO
     ═══════════════════════════════════════════════════════════════ -->
<section class="ply-cmp-hero">
  <div class="ply-inner">
    <div class="ply-cmp-hero__eyebrow">
      <span class="ply-cmp-hero__pill">Wren AI</span>
      <span class="ply-cmp-hero__sep">vs</span>
      <span class="ply-cmp-hero__pill ply-cmp-hero__pill--accent">PlyDB</span>
    </div>
    <h1>Wren AI vs <em>PlyDB</em></h1>
    <p class="ply-cmp-hero__sub">
      Wren AI and PlyDB share a lot on the surface: both are open source under Apache 2.0, both are read-only, and both connect AI agents to databases via native MCP servers. The difference is the layer in between. Wren AI translates natural language into SQL through a governed semantic model (MDL) — the goal is letting business users ask questions without writing SQL. PlyDB gives AI agents direct SQL access with no translation layer — the goal is letting agents that already write SQL reach live data as simply as possible.
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
            <th>Wren AI</th>
            <th class="ply-cmp-th--ply">PlyDB</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>Natural language to SQL</td>
            <td><span class="ply-cmp-val">Built-in — Wren AI translates natural language to SQL through its MDL semantic layer</span></td>
            <td><span class="ply-cmp-val">Any AI agent connected to PlyDB handles natural language and writes the SQL directly</span></td>
          </tr>
          <tr>
            <td>Semantic context</td>
            <td><span class="ply-cmp-val">MDL — engineer-authored model defines entities, metrics, and business logic upfront; enforced consistently across all queries</span></td>
            <td><span class="ply-cmp-val">OSI overlays — agents discover schema automatically and write context incrementally across sessions; no upfront modeling required</span></td>
          </tr>
          <tr>
            <td>Charts &amp; report generation</td>
            <td><span class="ply-cmp-win">Auto-generated charts, dashboards, and spreadsheet exports alongside query results</span></td>
            <td><span class="ply-cmp-val">Not applicable — returns query results; visualization is handled by the agent or downstream tools</span></td>
          </tr>
          <tr>
            <td>Business user self-service</td>
            <td><span class="ply-cmp-val">Natural language UI built-in — business users ask questions directly without an agent</span></td>
            <td><span class="ply-cmp-val">Any AI agent (Claude, GPT, Gemini) can serve as the natural language interface — no SQL knowledge required from the user</span></td>
          </tr>
          <tr>
            <td>Agent integration (MCP)</td>
            <td><span class="ply-cmp-val">Native MCP — Wren Engine exposes the semantic layer to Claude, Cline, Cursor, and other agents</span></td>
            <td><span class="ply-cmp-val">Native MCP &amp; CLI — direct SQL access to configured sources</span></td>
          </tr>
          <tr>
            <td>Direct SQL access</td>
            <td><span class="ply-cmp-val">Queries route through the MDL semantic layer — suited to governed, modeled data</span></td>
            <td><span class="ply-cmp-val">Agents construct and execute arbitrary SQL directly against live sources</span></td>
          </tr>
          <tr>
            <td>Cross-source queries</td>
            <td><span class="ply-cmp-val">Single source per project — cross-source requires a Trino integration as a workaround</span></td>
            <td><span class="ply-cmp-win">JOIN across any connected source in one query — no workaround needed</span></td>
          </tr>
          <tr>
            <td>Read-only</td>
            <td><span class="ply-cmp-val">Read-only by design</span></td>
            <td><span class="ply-cmp-val">Read-only by design</span></td>
          </tr>
          <tr>
            <td>Open source</td>
            <td><span class="ply-cmp-val">Apache 2.0</span></td>
            <td><span class="ply-cmp-val">Apache 2.0</span></td>
          </tr>
          <tr>
            <td>Cost</td>
            <td><span class="ply-cmp-val">Free tier (20 credits/month); paid plans from $99/month; enterprise on request</span></td>
            <td><span class="ply-cmp-win">Open source — Apache 2.0</span></td>
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
        <div class="ply-cmp-def-card__label">Wren AI</div>
        <h3>Text-to-SQL GenBI Agent</h3>
        <p>Wren AI is an open-source Generative BI agent built around a governed semantic layer. Data engineers define their data model in MDL (Model Definition Language) — a JSON format that encodes entities, metrics, dimensions, and business rules — and Wren AI uses that model to translate natural language questions into accurate SQL. Business users ask questions; Wren AI writes and executes the SQL and returns results alongside auto-generated charts and dashboards. A native MCP server exposes the semantic layer to AI agents via Claude, Cline, and Cursor. Wren AI connects to PostgreSQL, MySQL, BigQuery, Snowflake, DuckDB, Clickhouse, and more — but each project is scoped to a single data source; cross-source queries require a Trino integration. Wren AI Cloud is available for teams who don't want to self-host.</p>
      </div>
      <div class="ply-cmp-def-card ply-cmp-def-card--ply">
        <div class="ply-cmp-def-card__label">PlyDB</div>
        <h3>AI Agent Database Gateway</h3>
        <p>PlyDB is an open-source gateway built from the ground up for AI agents that already write SQL. You declare your data sources in a single JSON config file — PostgreSQL, MySQL, SQLite, S3, files, Google Sheets — and any AI agent connects immediately via native MCP or CLI, with no semantic translation layer in the way. PlyDB's semantic context system works differently from MDL: it auto-discovers schema and provides an OSI-format overlay system where agents themselves record institutional knowledge — enum meanings, business rules, domain context — that persists and compounds across sessions. No upfront modeling phase; agents build understanding incrementally as they query. Read-only by design, single binary, minutes to first query.</p>
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
        <h3>Choose Wren AI when&hellip;</h3>
        <ul class="ply-cmp-when-list">
          <li>Business users need to ask data questions in plain language — no SQL, no agent configuration required</li>
          <li>Metric governance matters — "Revenue" and "Active Users" should mean the same thing in every query, enforced by MDL</li>
          <li>Auto-generated charts and dashboards are a requirement alongside query results</li>
          <li>You have a single primary data source and want a fast, governed natural language interface on top of it</li>
          <li>Multilingual query support matters — Wren AI handles 10+ languages natively</li>
        </ul>
      </div>
      <div class="ply-cmp-when-card ply-cmp-when-card--ply">
        <h3>Choose PlyDB when&hellip;</h3>
        <ul class="ply-cmp-when-list">
          <li>Your AI agent already writes SQL — you need a data access layer, not a translation layer on top of it</li>
          <li>You need to JOIN across multiple data sources in a single query — Wren AI is scoped to one source per project</li>
          <li>Semantic context should grow from agent conversations, not require upfront MDL modeling before the first query</li>
          <li>You want zero infrastructure overhead — single binary, no Docker stack, no LLM service to configure</li>
          <li>No per-query credit system — PlyDB has no usage caps or metered tiers</li>
        </ul>
      </div>
    </div>
    <p style="margin-top: 2rem; font-size: 0.9rem; color: var(--ply-text-dim); line-height: 1.65; max-width: 700px;">
      <strong style="color: var(--ply-text);">These tools can complement each other.</strong> Wren AI handles the human-facing layer — business users asking natural language questions against a governed semantic model. PlyDB handles the agent layer — giving AI agents direct SQL access to live operational databases, flat files, and sources that don't belong in a semantic model. Both are read-only, both are Apache 2.0, and both can run alongside each other in the same stack.
    </p>
  </div>
</section>

<hr class="ply-divider">

<!-- ═══════════════════════════════════════════════════════════════
     FOOTER CTA
     ═══════════════════════════════════════════════════════════════ -->
<section class="ply-footer-cta">
  <div class="ply-inner">
    <h2>Direct SQL access. No translation layer.</h2>
    <p>Your agent writes the SQL. PlyDB connects it to your data. No modeling phase. No credits. Open source.</p>
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
