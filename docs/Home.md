# nodejs-project-arch

AI-friendly Node.js project architecture standards for OpenClaw/Codex agents.

## What is this?

A skill that teaches AI agents how to structure Node.js projects so that:
- Files stay small enough to read without blowing the context window
- Config is externalized for hot-reload via admin dashboards
- Code is modular and easy to navigate

## Problem

When AI agents work with large single-file codebases:
- A 3000-line `server.js` consumes ~40K tokens per read (20% of 200K context)
- 3-5 rounds of conversation triggers context compression
- Agent loses track of previous discussion

## Solution

Split files by function, each under 400 lines:
- A 200-line module consumes ~2.7K tokens (1.3% of context)
- 10-15 productive rounds before compression
- 70-93% token savings per file read

## Supported Project Types

| Type | Examples |
|------|----------|
| H5 Games | Canvas games, Phaser, Matter.js physics |
| Data Tools | Web scrapers, data analyzers, sync tools |
| Content Platforms | CMS, generators, publishers |
| Dashboards | Monitoring, analytics, admin panels |
| API Services | REST APIs, microservices |
| SDK Libraries | Shared modules consumed by multiple projects |

## Quick Links

- [Installation](Installation.md)
- [Game Architecture](Game-Architecture.md)
- [Tool Architecture](Tool-Architecture.md)
- [SDK Architecture](SDK-Architecture.md)
- [Config Pattern](Config-Pattern.md)
- [Admin Dashboard](Admin-Dashboard.md)
- [Token Savings](Token-Savings.md)
