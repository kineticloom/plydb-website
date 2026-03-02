---
title: FAQ
weight: 10
---

## Do I need to write SQL myself or can my AI agent do that?

While you are welcome to use PlyDB with hand-crafted SQL, AI agents can be very
good at writing SQL too — exploring, understanding, and analyzing datasets —
even if your data is
[less organized than you'd like](#how-organized-should-my-data-be).

## Should I use MCP or CLI?

It depends on your workflow and your agent's capabilities. See the
[MCP vs CLI comparison](/docs/agent-integration/#mcp-vs-cli) for a detailed
breakdown.

In short: the CLI approach is more dynamic — agents can reconfigure PlyDB
between calls, changing config files or evolving semantic context overlays on
the fly. MCP requires a server restart to pick up configuration changes.

Your choice may also be informed by your agent's limitations around shell
access. For example, OpenClaw allows very permissive system access, while Claude
Cowork operates in an isolated sandbox that limits tools and networking, and
Claude Code is somewhere in the middle.

{{< callout type="tip" >}} When first setting up or evolving your configuration,
we recommend pairing a CLI-capable agent with PlyDB. This lets the agent
actively assist with data source setup and capture semantics into a context
overlay. Once your configuration has stabilized, transition to whichever method
works best for your preferred agent. {{< /callout >}}

## How organized should my data be?

Even if your data isn't well organized, you may be pleasantly surprised at how
capable AI agents are at making sense of it. We suggest you give it a shot!

{{< callout type="tip" >}} After a session of data analysis, ask your AI agent
to distill its learnings about your data's semantics and
[write a semantic context overlay](/docs/semantic-context/#overlays) to record
its findings for future sessions. {{< /callout >}}

## Can I have an AI agent write my PlyDB config file for me?

Yes! Install the [PlyDB Agent Skill](/docs/agent-integration/#cli-agent-skill)
or point your agent at the [config file documentation](/docs/data-sources/) and
tell it about the data sources you want to configure.

## Can I have an AI agent write my semantic context overlays for me?

Yes! Install the [PlyDB Agent Skill](/docs/agent-integration/#cli-agent-skill)
to teach your agent about PlyDB
[semantic context overlays](/docs/semantic-context/#overlays).

Try asking your agent to write an overlay file after a data analysis session —
it's a particularly good opportunity to capture learnings for future sessions.

## Does the PlyDB CLI work with Claude Code?

Yes. Claude Code can run any tool available on your system (with your
permission), so there are no sandboxing restrictions. Both relative and absolute
paths work.

## Does the PlyDB CLI work with Claude Cowork?

Yes, but with some limitations:

- The `plydb` binary must be in a directory you've granted Claude access to
  (e.g. your project workspace).
- Network access is restricted from within the sandbox, so PlyDB cannot connect
  to networked data sources (PostgreSQL, MySQL, S3, Google Sheets) or download
  extensions for its data connectors.

For local file sources (CSV, JSON, etc.), the CLI works fine. For networked data
sources, use [MCP](/docs/agent-integration/#mcp) instead.
