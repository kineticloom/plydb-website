---
title: "PlyDB vs LangChain: Data Gateway vs. Agent Framework"
description: "LangChain is a framework for building AI agents and applications — LLM orchestration, RAG, tool use, and LangGraph for stateful multi-agent workflows. PlyDB is the data infrastructure those agents connect to. They solve different layers of the same problem."
---

<div class="ply-compare">

<!-- ═══════════════════════════════════════════════════════════════
     HERO
     ═══════════════════════════════════════════════════════════════ -->
<section class="ply-cmp-hero">
  <div class="ply-inner">
    <div class="ply-cmp-hero__eyebrow">
      <span class="ply-cmp-hero__pill">LangChain</span>
      <span class="ply-cmp-hero__sep">vs</span>
      <span class="ply-cmp-hero__pill ply-cmp-hero__pill--accent">PlyDB</span>
    </div>
    <h1>LangChain vs <em>PlyDB</em></h1>
    <p class="ply-cmp-hero__sub">
      LangChain and PlyDB are not alternatives — they operate at different layers of the agent stack. LangChain is a framework for building AI agents: LLM orchestration, tool use, RAG, memory, and stateful multi-agent workflows via LangGraph. PlyDB is the data infrastructure those agents connect to: a read-only gateway that gives any agent live SQL access to databases, files, and cloud sources via MCP. The comparison that matters is between LangChain's built-in SQL tools and using PlyDB as a dedicated data gateway instead.
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
            <th>LangChain</th>
            <th class="ply-cmp-th--ply">PlyDB</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>Agent &amp; application framework</td>
            <td><span class="ply-cmp-win">Core purpose — LLM orchestration, tool use, chains, and stateful multi-agent workflows via LangGraph</span></td>
            <td><span class="ply-cmp-val">Not applicable — PlyDB is data infrastructure, not an agent framework</span></td>
          </tr>
          <tr>
            <td>RAG &amp; vector retrieval</td>
            <td><span class="ply-cmp-win">First-class support — 200+ document loaders, vector store integrations, advanced retrieval patterns</span></td>
            <td><span class="ply-cmp-val">Not applicable — PlyDB queries structured data sources; no vector retrieval</span></td>
          </tr>
          <tr>
            <td>LLM &amp; model integrations</td>
            <td><span class="ply-cmp-win">Hundreds of LLM providers — OpenAI, Anthropic, Google, Bedrock, Ollama, and more</span></td>
            <td><span class="ply-cmp-val">Not applicable — PlyDB is model-agnostic data access infrastructure</span></td>
          </tr>
          <tr>
            <td>Observability &amp; tracing (LangSmith)</td>
            <td><span class="ply-cmp-win">LangSmith — full trace visibility, evaluation, cost tracking, and prompt management</span></td>
            <td><span class="ply-cmp-val">Not applicable</span></td>
          </tr>
          <tr>
            <td>MCP server (agents connect to it)</td>
            <td><span class="ply-cmp-val">LangChain consumes MCP servers via adapters — it is an MCP client, not a server</span></td>
            <td><span class="ply-cmp-win">First-party MCP server — any agent connects to PlyDB as a data source with zero custom code</span></td>
          </tr>
          <tr>
            <td>SQL database access</td>
            <td><span class="ply-cmp-val">SQLDatabaseChain / SQL agent via SQLAlchemy — single database per chain, NL to SQL</span></td>
            <td><span class="ply-cmp-val">Declarative multi-source config — direct SQL access to any connected source</span></td>
          </tr>
          <tr>
            <td>Cross-source queries</td>
            <td><span class="ply-cmp-val">No native federation — each chain connects to one database; cross-source requires custom orchestration</span></td>
            <td><span class="ply-cmp-win">JOIN across any connected source in one query</span></td>
          </tr>
          <tr>
            <td>Semantic context for agents</td>
            <td><span class="ply-cmp-val">Agent memory and vector stores — retrieved per query, not accumulated as structured schema overlays</span></td>
            <td><span class="ply-cmp-win">Auto-discovery + OSI overlays that compound across sessions</span></td>
          </tr>
          <tr>
            <td>Credential management</td>
            <td><span class="ply-cmp-val">Per-integration setup — credentials configured per chain or tool, no unified config</span></td>
            <td><span class="ply-cmp-win">Env var indirection — one config file, credentials never hard-coded</span></td>
          </tr>
          <tr>
            <td>Read-only safety</td>
            <td><span class="ply-cmp-val">SQL agent can execute writes — read-only requires explicit constraint in the prompt or connection config</span></td>
            <td><span class="ply-cmp-win">Read-only by design — no configuration needed to prevent agents from writing</span></td>
          </tr>
          <tr>
            <td>Time to first live data query</td>
            <td><span class="ply-cmp-val">Build a chain, configure credentials, handle schema introspection, wire up tools</span></td>
            <td><span class="ply-cmp-win">Minutes — single binary, one JSON config file, MCP endpoint ready</span></td>
          </tr>
          <tr>
            <td>Open source</td>
            <td><span class="ply-cmp-val">MIT (LangChain, LangGraph); LangSmith is proprietary SaaS</span></td>
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
        <div class="ply-cmp-def-card__label">LangChain</div>
        <h3>AI Agent &amp; Application Framework</h3>
        <p>LangChain is an open-source framework for building AI-powered applications and agents. It provides the orchestration layer: LLM integrations across hundreds of providers, tool use, memory, chains, and 200+ document loaders for ingesting data into RAG pipelines. LangGraph extends this into stateful, graph-based multi-agent workflows with cycles, branching, parallel execution, and persistent state — the recommended approach for production agents. LangSmith adds observability: full trace visibility, evaluation, cost tracking, and prompt management. For database access, LangChain provides a SQL agent via SQLAlchemy that translates natural language to SQL — but it connects to one database per chain with no cross-source federation or semantic context system. LangChain acts as an MCP client, consuming tools from MCP servers like PlyDB rather than exposing one itself.</p>
      </div>
      <div class="ply-cmp-def-card ply-cmp-def-card--ply">
        <div class="ply-cmp-def-card__label">PlyDB</div>
        <h3>AI Agent Database Gateway</h3>
        <p>PlyDB is an open-source gateway built from the ground up for AI agents. You declare your data sources in a single JSON config file — PostgreSQL, MySQL, SQLite, S3, files, Google Sheets — and any AI agent connects immediately via native MCP or CLI, read-only by design. PlyDB's semantic context system auto-discovers schema and provides an OSI-format overlay system where agents record institutional knowledge — enum meanings, business rules, domain context — that persists and compounds across sessions. Where LangChain's SQL agent requires per-chain database setup with no federation or credential system, PlyDB provides a production-ready data gateway that any agent — including LangChain and LangGraph agents — can connect to as a single MCP endpoint covering all configured sources.</p>
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
        <h3>Use LangChain when&hellip;</h3>
        <ul class="ply-cmp-when-list">
          <li>You're building an AI agent or application from scratch — orchestrating LLMs, tools, memory, and multi-step workflows</li>
          <li>RAG over documents is a core requirement — 200+ loaders, vector stores, and advanced retrieval patterns</li>
          <li>You need stateful, graph-based agent workflows with branching, retries, and parallel execution (LangGraph)</li>
          <li>Observability matters — LangSmith gives full trace visibility into every LLM call and agent decision</li>
          <li>You want maximum control over the agent loop and don't need a dedicated data gateway layer</li>
        </ul>
      </div>
      <div class="ply-cmp-when-card ply-cmp-when-card--ply">
        <h3>Use PlyDB when&hellip;</h3>
        <ul class="ply-cmp-when-list">
          <li>Your agent (whether LangChain, Claude, GPT, or anything else) needs live access to databases — and you want that solved by a dedicated gateway, not a hand-rolled SQL chain</li>
          <li>You need cross-source queries — your agent needs to JOIN across databases that LangChain's SQL agent would require separate chains to reach</li>
          <li>Read-only access should be the enforced default — not something you have to prompt-engineer your way to</li>
          <li>Semantic context should compound across sessions without requiring a vector store and retrieval pipeline</li>
          <li>You want a single MCP endpoint covering all your data sources instead of configuring database credentials per agent or chain</li>
        </ul>
      </div>
    </div>
    <p style="margin-top: 2rem; font-size: 0.9rem; color: var(--ply-text-dim); line-height: 1.65; max-width: 700px;">
      <strong style="color: var(--ply-text);">These tools are designed to work together.</strong> LangChain and LangGraph build the agent; PlyDB is the MCP data source the agent connects to. Rather than wiring a LangChain SQL agent directly to each database, teams use PlyDB as a single, production-ready data gateway — and their LangChain agents consume it like any other MCP tool.
    </p>
  </div>
</section>

<hr class="ply-divider">

<!-- ═══════════════════════════════════════════════════════════════
     FOOTER CTA
     ═══════════════════════════════════════════════════════════════ -->
<section class="ply-footer-cta">
  <div class="ply-inner">
    <h2>The data gateway your LangChain agent needs.</h2>
    <p>One MCP endpoint. Every source. Read-only. Semantic context that compounds. Open source.</p>
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
