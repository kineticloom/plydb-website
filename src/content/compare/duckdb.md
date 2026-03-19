---
title: "PlyDB vs DuckDB: What PlyDB Adds on Top of DuckDB's Query Engine"
description: "PlyDB is built on DuckDB. If you're considering raw DuckDB for AI agent data access, PlyDB is the AI-native layer you'd build on top of it anyway — native MCP, declarative config, semantic context, and Google Sheets integration included."
---

<div class="ply-compare">

<!-- ═══════════════════════════════════════════════════════════════
     HERO
     ═══════════════════════════════════════════════════════════════ -->
<section class="ply-cmp-hero">
  <div class="ply-inner">
    <div class="ply-cmp-hero__eyebrow">
      <span class="ply-cmp-hero__pill">DuckDB</span>
      <span class="ply-cmp-hero__sep">vs</span>
      <span class="ply-cmp-hero__pill ply-cmp-hero__pill--accent">PlyDB</span>
    </div>
    <h1>DuckDB vs <em>PlyDB</em></h1>
    <p class="ply-cmp-hero__sub">
      PlyDB is built on DuckDB. The two share the same query engine, the same cross-source federation model, and the same SQL dialect. Where they differ: DuckDB is an embeddable library for analysts and engineers who want to build with it directly. PlyDB is the AI-native layer on top — adding the MCP server, declarative configuration, semantic context system, and Google Sheets integration that DuckDB doesn't ship.
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
            <th>DuckDB</th>
            <th class="ply-cmp-th--ply">PlyDB</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>Query engine</td>
            <td><span class="ply-cmp-val">DuckDB — columnar, vectorized, analytical</span></td>
            <td><span class="ply-cmp-val">DuckDB — same engine, same performance</span></td>
          </tr>
          <tr>
            <td>Embedded library (Python, Go, Rust, JS)</td>
            <td><span class="ply-cmp-val">In-process — runs inside your application, no server</span></td>
            <td><span class="ply-cmp-val">Runs as a separate process — not an embeddable library</span></td>
          </tr>
          <tr>
            <td>Write operations</td>
            <td><span class="ply-cmp-val">Full read/write — INSERT, UPDATE, DELETE supported</span></td>
            <td><span class="ply-cmp-val">Read-only by design — safe default for AI agents</span></td>
          </tr>
          <tr>
            <td>Extension ecosystem access</td>
            <td><span class="ply-cmp-win">Direct access to all 28+ core extensions (Iceberg, Delta, spatial, etc.)</span></td>
            <td><span class="ply-cmp-val">Configured sources only — not all DuckDB extensions exposed</span></td>
          </tr>
          <tr>
            <td>Browser / WASM</td>
            <td><span class="ply-cmp-win">DuckDB-Wasm runs in the browser — no server required</span></td>
            <td><span class="ply-cmp-val">Not applicable</span></td>
          </tr>
          <tr>
            <td>MCP server (agent integration)</td>
            <td><span class="ply-cmp-val">No native MCP — build it yourself or use a community workaround</span></td>
            <td><span class="ply-cmp-win">First-party MCP server — ships with PlyDB, zero custom code</span></td>
          </tr>
          <tr>
            <td>Multi-source configuration</td>
            <td><span class="ply-cmp-val">Manual ATTACH SQL statements per source — written in code or scripts</span></td>
            <td><span class="ply-cmp-win">Declarative JSON config — add a source, add a block</span></td>
          </tr>
          <tr>
            <td>Credential management</td>
            <td><span class="ply-cmp-val">Inline credentials or DuckDB secrets store — manual per source</span></td>
            <td><span class="ply-cmp-win">Env var indirection — config files are safe to commit</span></td>
          </tr>
          <tr>
            <td>Semantic context for agents</td>
            <td><span class="ply-cmp-val">INFORMATION_SCHEMA only — raw structure, no semantic layer</span></td>
            <td><span class="ply-cmp-win">Auto-discovery + OSI overlays that compound across sessions</span></td>
          </tr>
          <tr>
            <td>License</td>
            <td><span class="ply-cmp-val">MIT</span></td>
            <td><span class="ply-cmp-val">Apache 2.0</span></td>
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
        <div class="ply-cmp-def-card__label">DuckDB</div>
        <h3>In-Process Analytical Query Engine</h3>
        <p>DuckDB is a fast, embeddable OLAP database — think SQLite, but purpose-built for analytical queries. It runs in-process inside Python scripts, Go services, Rust applications, Node.js, and even the browser via WASM. With 28+ core extensions, it queries Parquet, CSV, JSON, Delta Lake, Iceberg, S3, PostgreSQL, MySQL, and SQLite natively — and can JOIN across all of them in a single query. It is MIT-licensed, has no server to run, and installs in seconds. DuckDB ships no MCP server, no semantic context system, and no declarative config layer. Those are things you build on top of it.</p>
      </div>
      <div class="ply-cmp-def-card ply-cmp-def-card--ply">
        <div class="ply-cmp-def-card__label">PlyDB</div>
        <h3>AI Agent Database Gateway</h3>
        <p>PlyDB is the AI-native layer built on top of DuckDB. It uses DuckDB as its query engine and inherits its cross-source federation, analytical performance, and SQL expressiveness. On top of that foundation, PlyDB adds what DuckDB doesn't ship: a native MCP server so any AI agent connects without custom code, a declarative JSON config that replaces manual ATTACH statements, env-var-based credential management, and a semantic context system — automatic schema discovery feeding into OSI-format overlays that agents write and accumulate across sessions. If you were going to use DuckDB to power an AI agent gateway, PlyDB is what you would build.</p>
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
        <h3>Use raw DuckDB when&hellip;</h3>
        <ul class="ply-cmp-when-list">
          <li>You're embedding analytical queries inside a Python script, Go service, or Rust application — in-process, no server</li>
          <li>You need write access — INSERT, UPDATE, DELETE — to DuckDB or connected databases</li>
          <li>You want direct access to extensions PlyDB doesn't expose: spatial, Iceberg table creation, DuckLake, Lance, and others</li>
          <li>You're building in the browser and need DuckDB-Wasm</li>
          <li>You prefer to wire up your own MCP, config, and credential layers</li>
        </ul>
      </div>
      <div class="ply-cmp-when-card ply-cmp-when-card--ply">
        <h3>Use PlyDB when&hellip;</h3>
        <ul class="ply-cmp-when-list">
          <li>You want an AI agent connected to your databases in minutes — no MCP server to build, no ATTACH statements to write</li>
          <li>Semantic context matters — you want agents to accumulate institutional knowledge across sessions, not see raw INFORMATION_SCHEMA</li>
          <li>You want a read-only gateway by default — safe for agents that shouldn't be able to modify data</li>
          <li>Config files need to be commitable — env var credential indirection keeps secrets out of your repository</li>
        </ul>
      </div>
    </div>
    <p style="margin-top: 2rem; font-size: 0.9rem; color: var(--ply-text-dim); line-height: 1.65; max-width: 700px;">
      <strong style="color: var(--ply-text);">These tools are complementary by design.</strong> PlyDB is a DuckDB application. The same analytical power, the same cross-source SQL — with the AI-native plumbing already built. Teams that use DuckDB directly for pipelines and scripts can deploy PlyDB alongside it as the agent-facing gateway to the same sources.
    </p>
  </div>
</section>

<hr class="ply-divider">

<!-- ═══════════════════════════════════════════════════════════════
     FOOTER CTA
     ═══════════════════════════════════════════════════════════════ -->
<section class="ply-footer-cta">
  <div class="ply-inner">
    <h2>DuckDB's power. The AI-native layer included.</h2>
    <p>MCP server. Declarative config. Semantic context. All on top of DuckDB — free and open source.</p>
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
