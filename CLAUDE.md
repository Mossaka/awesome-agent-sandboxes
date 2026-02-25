# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Purpose

Curated "awesome list" of ~80 sandboxing technologies for AI agents — containers, microVMs, WebAssembly, OS-level isolation, language runtimes, and cloud platforms. Serves as a landscape overview for anyone building, deploying, or researching agent sandboxing.

## Repository Structure

- `README.md` — The main awesome list. ~80 curated entries organized by isolation mechanism with a Landscape Overview section covering industry trends.
- `getting-started.md` — Installation and quickstart guides for the top 15 sandboxes, with a comparison matrix and "Choosing the Right Sandbox" guide.
- `research/` — Raw research files from the initial data gathering phase (6 category-specific research files + 2 verified/deduplicated files). These are reference material, not user-facing.
- `CLAUDE.md` — This file.

## Entry Format

Each entry in README.md follows this pattern:
```
- [🤖] **[Name](url)** - Description. `stars: Xk` `language`
  - One line about agent relevance
```
- ⚠️ marks archived/superseded projects

## Categories

1. **Agent-Native Sandbox Platforms** (Open Source + Cloud/Proprietary)
2. **MicroVM & Hypervisor-Based** (VMMs + Unikernels)
3. **Container-Based** (Engines/Runtimes + Agent-Specific)
4. **WebAssembly-Based** (Runtimes + Agent Sandboxes + Standards)
5. **OS-Level & Process Isolation** (Tools + LSMs)
6. **Language Runtime Sandboxes** (JS/TS + Python + Code Execution Engines)
7. **AI Coding Agents & Frameworks** (agents with notable sandbox integration)
8. **Kubernetes-Native**
9. **Confidential Computing & TEEs**
10. **Novel & Research** (Experimental + Academic Papers)

## Contribution Guidelines

- Include: project name with link, GitHub stars, language, brief description, and one line about agent relevance.
- Categorize by underlying isolation mechanism.
- Verify the project exists and is not abandoned before adding.
- Focus on properties that matter for agent sandboxing: startup latency, resource overhead, security boundary strength, API surface (MCP, SDK, REST), language/runtime support, and orchestration.
- Filter out projects with <50 GitHub stars unless they offer genuinely novel approaches.
- Mark archived projects with ⚠️.
- Don't list foundational building blocks already subsumed by higher-level tools (e.g., runc is inside Docker, libseccomp is inside everything). Each entry should stand on its own.

## Key Landscape Trends (as of Feb 2026)

- Firecracker microVMs dominate cloud agent sandboxing (E2B, Fly.io, Blaxel, Docker Sandboxes)
- MCP (Model Context Protocol) is converging as the standard agent-to-sandbox interface
- Landlock LSM + Seatbelt are the OS-level primitives for CLI agent sandboxing (Codex, Cursor, nono)
- gVisor is the Kubernetes standard (GKE Autopilot, kubernetes-sigs/agent-sandbox, Modal)
- Wasm gaining traction for fine-grained tool sandboxing (Wassette, Pydantic Monty)
