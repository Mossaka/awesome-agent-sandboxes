# Verified Application-Level Sandboxes (Wasm + Platforms + Runtimes)

> Verification completed: 2026-02-25
> Sources: wasm-research.md, platform-research.md, runtime-research.md

---

## Duplicates Found

- **mcp-run-python (Pydantic)**: Listed in both wasm-research.md (Tier 1) and runtime-research.md (Python Sandboxes). Note: This project is **ARCHIVED** (Jan 30, 2026) — superseded by Monty. Should only appear once, under archived/historical.
- **LangChain Sandbox**: Referenced in both platform-research.md and runtime-research.md. **ARCHIVED** (Jan 14, 2026) — superseded by DeepAgents. Deduplicate to archived/historical.
- **Pyodide**: Explicitly listed in runtime-research.md and heavily referenced as foundation in wasm-research.md (mcp-run-python) and platform-research.md (smolagents, Cohere Terrarium). Keep in runtime category only.
- **Cloudflare workerd** (wasm-research.md) vs **Cloudflare Sandbox SDK** (platform-research.md): Different products (workerd is the runtime engine, Sandbox SDK is the agent-facing API). Keep both but note the relationship.
- **Deno** + **Deno Sandbox**: Listed as two entries in runtime-research.md. Deno is the runtime, Deno Sandbox is the cloud product. Merge into one entry.
- **bolt.new** + **bolt.diy**: Both in platform-research.md. bolt.diy is the community fork of bolt.new. Keep as separate entries since bolt.diy has more stars and broader LLM support.

---

## Verified Entries

### Category: WebAssembly

#### Wassette (Microsoft)
- **Repo**: https://github.com/microsoft/wassette
- **Stars**: 846 (verified)
- **Description**: Security-oriented runtime that executes WebAssembly Components via MCP, built on Wasmtime sandbox with browser-grade isolation.
- **Agent Relevance**: Direct
- **Verification**: README explicitly discusses AI agents, MCP, sandbox security. Works with GitHub Copilot, Claude Code, Cursor, Gemini CLI. Latest release v0.4.0 (Feb 2026).
- **Status**: Active
- **Include in final list**: Yes

#### amla-sandbox
- **Repo**: https://github.com/amlalabs/amla-sandbox
- **Stars**: 309 (verified)
- **Description**: WASM sandbox with capability enforcement for AI agents — QuickJS inside WASM with controlled tool access.
- **Agent Relevance**: Direct
- **Verification**: README explicitly targets agent frameworks (LangChain, AutoGen, SWE-Agent). Capability-based security with unforgeable tokens. Latest release v0.1.7 (Jan 2026).
- **Status**: Active
- **Include in final list**: Yes

#### Wasmtime (Bytecode Alliance)
- **Repo**: https://github.com/bytecodealliance/wasmtime
- **Stars**: 17,600 (verified)
- **Description**: Fast, secure, standards-compliant WebAssembly runtime with Cranelift JIT/AOT compilation.
- **Agent Relevance**: Indirect (foundation layer)
- **Verification**: Underlies Wassette, amla-sandbox, and many other agent sandboxes. v42.0.0 (Feb 2026). Does not mention AI directly but is the security foundation.
- **Status**: Active
- **Include in final list**: Yes

#### WasmEdge (CNCF)
- **Repo**: https://github.com/WasmEdge/WasmEdge
- **Stars**: 10,500 (verified; research said ~11,470)
- **Description**: Lightweight Wasm runtime — CNCF Sandbox project with LlamaEdge for running LLMs on device.
- **Agent Relevance**: Indirect (with direct AI extensions via LlamaEdge)
- **Verification**: README mentions LLM inference, sandboxed execution, wasi-nn support. Active CNCF project.
- **Status**: Active
- **Include in final list**: Yes

#### Wasmer
- **Repo**: https://github.com/wasmerio/wasmer
- **Stars**: 20,400 (verified)
- **Description**: Fast, secure WebAssembly runtime — no file, network, or environment access unless explicitly enabled.
- **Agent Relevance**: Indirect
- **Verification**: Strong sandbox-by-default. No direct AI/agent tooling. v7.0.1 (Feb 2026).
- **Status**: Active
- **Include in final list**: Yes

#### Spin (Fermyon/Akamai)
- **Repo**: https://github.com/spinframework/spin
- **Stars**: 6,300 (verified; research said ~5,200)
- **Description**: Framework for building fast, secure, composable cloud microservices with WebAssembly. CNCF Sandbox project.
- **Agent Relevance**: Indirect (with emerging agent support)
- **Verification**: README mentions "Serverless AI" as supported API. Sub-millisecond cold starts. CNCF project, Spin 3.x, acquired by Akamai.
- **Status**: Active
- **Include in final list**: Yes

#### Extism
- **Repo**: https://github.com/extism/extism
- **Stars**: 5,400 (verified)
- **Description**: Universal WebAssembly plugin framework — execute untrusted code safely via Wasm plugins.
- **Agent Relevance**: Indirect
- **Verification**: Designed for executing arbitrary untrusted code. 16+ host language SDKs. No direct AI agent tooling.
- **Status**: Active
- **Include in final list**: Yes

#### wazero
- **Repo**: https://github.com/tetratelabs/wazero
- **Stars**: 5,100 (verified)
- **Description**: Zero-dependency WebAssembly runtime for Go — pure Go, no CGO.
- **Agent Relevance**: Indirect
- **Verification**: Foundation for Go-based Wasm sandboxing. No direct agent use.
- **Status**: Active
- **Include in final list**: Yes

#### workerd (Cloudflare)
- **Repo**: https://github.com/cloudflare/workerd
- **Stars**: 7,500 (verified)
- **Description**: V8 isolate-based JavaScript/Wasm runtime powering Cloudflare Workers.
- **Agent Relevance**: Indirect
- **Verification**: Powers the infrastructure behind Cloudflare Sandbox SDK. Sub-millisecond cold starts. WASI support.
- **Status**: Active
- **Include in final list**: Yes

#### wasmCloud (CNCF)
- **Repo**: https://github.com/wasmCloud/wasmCloud
- **Stars**: 2,200 (verified; research said ~1,900)
- **Description**: CNCF Incubating project for polyglot Wasm apps across cloud, K8s, or edge. Capability-based security.
- **Agent Relevance**: Indirect (with direct agent sandbox efforts via Cosmonic/Sandbox MCP)
- **Verification**: Cosmonic Sandbox MCP plugin built on wasmCloud for agent sandboxing. MCP Registry support confirmed. v1.9.2 (Feb 2026).
- **Status**: Active
- **Include in final list**: Yes

#### Golem Cloud
- **Repo**: https://github.com/golemcloud/golem
- **Stars**: 964 (verified)
- **Description**: Agent-native platform for building AI agents with durable computing on WebAssembly.
- **Agent Relevance**: Direct
- **Verification**: Self-describes as "agent-native platform." Blog post confirms pivot to agentic applications. Durable execution (state survives failures). MCP support. Tags: "agentic-ai", "ai-agent-runtime".
- **Status**: Active
- **Include in final list**: Yes

#### WAMR (WebAssembly Micro Runtime)
- **Repo**: https://github.com/bytecodealliance/wasm-micro-runtime
- **Stars**: ~5,700 (not re-verified, trusted)
- **Description**: Lightweight Wasm runtime for embedded, IoT, edge, TEE, and cloud-native applications.
- **Agent Relevance**: Potential
- **Verification**: Not directly used for agent sandboxing. Small footprint + TEE support (SGX) relevant for resource-constrained deployments.
- **Status**: Active
- **Include in final list**: Yes (as a notable specialized runtime)

#### wasm3
- **Repo**: https://github.com/wasm3/wasm3
- **Stars**: 7,900 (verified)
- **Description**: Fast WebAssembly interpreter using M3 interpretation topology — runs on 64KB Flash / 10KB RAM.
- **Agent Relevance**: Potential
- **Verification**: No agent use. Extremely portable (microcontrollers). Minimal maintenance due to developer circumstances.
- **Status**: Minimal maintenance
- **Include in final list**: Yes (historically significant, unique niche)

#### Sandbox MCP (Cosmonic/wasmCloud)
- **Repo**: https://github.com/cosmonic-labs/openapi2mcp (related tooling)
- **Stars**: 2 (openapi2mcp repo only)
- **Description**: Free plugin for wasmCloud that generates MCP servers as secure WebAssembly components from OpenAPI specs.
- **Agent Relevance**: Direct
- **Verification**: openapi2mcp has only 2 stars. The product (sandboxmcp.ai) is real but the open-source footprint is minimal. Tightly coupled to wasmCloud ecosystem.
- **Status**: Active
- **Include in final list**: Yes (but note small OSS footprint; primarily a product)

#### WASI (WebAssembly System Interface)
- **Repo**: https://github.com/WebAssembly/WASI
- **Stars**: ~4,900 (not re-verified)
- **Description**: Capability-based system interface standard for WebAssembly — foundation for all Wasm sandboxing.
- **Agent Relevance**: Foundational
- **Verification**: WASI 0.2 stable, WASI 0.3 with async released Feb 2026. Foundation for all Wasm agent sandboxes.
- **Status**: Active
- **Include in final list**: Yes (as a standard/spec)

#### WebAssembly Component Model
- **Repo**: https://github.com/WebAssembly/component-model
- **Stars**: ~1,000 (not re-verified)
- **Description**: Higher-level format for composable, interoperable Wasm modules with typed interfaces.
- **Agent Relevance**: Foundational
- **Verification**: Enables composable agent tools. Used by Wassette, wasmCloud, Spin, Golem.
- **Status**: Active
- **Include in final list**: Yes (as a standard/spec)

#### llm-wasm-sandbox
- **Repo**: https://github.com/ThomasRohde/llm-wasm-sandbox
- **Stars**: 1 (verified)
- **Description**: Security sandbox for executing LLM-generated Python & JS using WebAssembly and WASI.
- **Agent Relevance**: Direct (by design)
- **Verification**: Purpose-built for LLM code execution with MCP support. However, only 1 star — no real adoption.
- **Status**: Active (but zero adoption)
- **Include in final list**: No (1 star, no adoption despite good design)

#### wasmi
- **Repo**: https://github.com/wasmi-labs/wasmi
- **Stars**: ~2,000 (not re-verified)
- **Description**: Lightweight WebAssembly interpreter for embedded systems.
- **Agent Relevance**: Potential
- **Verification**: No direct agent use. Used in Substrate/Polkadot blockchain.
- **Status**: Active
- **Include in final list**: No (too niche, no agent relevance)

#### WAVM
- **Repo**: https://github.com/WAVM/WAVM
- **Stars**: 2,800 (verified)
- **Description**: WebAssembly VM using LLVM for near-native performance.
- **Agent Relevance**: Potential
- **Verification**: No agent use. Status unclear — possibly unmaintained.
- **Status**: Unmaintained
- **Include in final list**: No (unmaintained, no agent relevance)

#### Lunatic
- **Repo**: https://github.com/lunatic-solutions/lunatic
- **Stars**: 4,800 (verified)
- **Description**: Erlang-inspired runtime for WebAssembly with preemptive scheduling and process isolation.
- **Agent Relevance**: Potential
- **Verification**: Interesting process model for multi-agent architectures, but last release May 2023. No agent use.
- **Status**: Stale
- **Include in final list**: No (stale, no agent relevance despite interesting design)

#### Enarx
- **Repo**: https://github.com/enarx/enarx
- **Stars**: 1,400 (verified)
- **Description**: Confidential Computing with WebAssembly — run Wasm in TEEs (Intel SGX, AMD SEV-SNP).
- **Agent Relevance**: Potential
- **Verification**: Relevant for privacy-sensitive agent workloads. Low activity but not archived.
- **Status**: Low activity
- **Include in final list**: Yes (unique niche — confidential computing + Wasm)

#### Veracruz
- **Repo**: https://github.com/veracruz-project/veracruz
- **Stars**: 198 (verified)
- **Description**: Privacy-preserving computation using Wasm in TEEs.
- **Agent Relevance**: Potential
- **Verification**: **ARCHIVED** (Feb 18, 2026). Transitioning to Emeritus stage.
- **Status**: Archived
- **Include in final list**: No (archived, low stars)

#### Timecraft
- **Repo**: https://github.com/stealthrocket/timecraft
- **Stars**: ~327 (not re-verified)
- **Description**: "The WebAssembly Time Machine" with time-travel debugging.
- **Agent Relevance**: Potential (archived)
- **Verification**: Archived Feb 2024. Team pivoted to Dispatch. Historically interesting.
- **Status**: Archived
- **Include in final list**: No (archived)

---

### Category: Agent Platforms & Cloud Sandboxes

#### E2B
- **Repo**: https://github.com/e2b-dev/E2B
- **Stars**: 11,000 (verified; research said ~10,900)
- **Description**: Open-source cloud runtime for AI agents with secure Firecracker microVM sandboxes.
- **Agent Relevance**: Direct
- **Verification**: Confirmed purpose-built for AI agents. Tags: "ai-agents", "sandbox". Used by Manus, Hugging Face. SDKs for Python/JS. 80ms startup.
- **Status**: Active
- **Include in final list**: Yes

#### Daytona
- **Repo**: https://github.com/daytonaio/daytona
- **Stars**: 60,400 (verified)
- **Description**: Secure, elastic infrastructure for running AI-generated code with sub-90ms sandbox creation.
- **Agent Relevance**: Direct
- **Verification**: Confirmed pivot to AI agent infrastructure (Feb 2025). Tags: "ai-agents", "sandbox". 159 releases, latest v0.146.0 (Feb 2026).
- **Status**: Active
- **Include in final list**: Yes

#### Modal Sandboxes
- **Repo/URL**: https://modal.com/products/sandboxes
- **Stars**: N/A (proprietary)
- **Description**: Python-first cloud platform with sandboxes for safely running AI-generated code at scale via gVisor.
- **Agent Relevance**: Direct
- **Verification**: Proprietary but well-documented. Used by Ramp, Quora/Poe, Codegen. SWE-bench support. Sub-second startup.
- **Status**: Active
- **Include in final list**: Yes

#### CodeSandbox SDK / Together Code Sandbox
- **Repo**: https://github.com/codesandbox/codesandbox-sdk
- **Stars**: 101 (verified; research said ~500 — significantly overestimated)
- **Description**: Programmatic API for microVM-based dev environments, now part of Together AI.
- **Agent Relevance**: Direct
- **Verification**: README confirms AI agent use case. MicroVM infrastructure with snapshot/restore. Acquired by Together AI.
- **Status**: Active
- **Include in final list**: Yes (despite low star count, Together AI acquisition validates significance)

#### Fly.io / Sprites
- **Repo/URL**: https://fly.io / https://sprites.dev
- **Stars**: N/A (proprietary)
- **Description**: Firecracker microVMs with Sprites providing persistent, stateful sandboxes for AI coding agents.
- **Agent Relevance**: Direct
- **Verification**: Sprites launched Jan 2026, confirmed purpose-built for AI agents (Claude Code, Gemini, Codex pre-installed). Checkpoint/restore ~300ms.
- **Status**: Active
- **Include in final list**: Yes

#### Blaxel
- **Repo/URL**: https://blaxel.ai
- **Stars**: N/A (proprietary)
- **Description**: Perpetual sandbox platform with <25ms resume for AI agents using microVM isolation.
- **Agent Relevance**: Direct
- **Verification**: Confirmed YC Spring 2025 batch. $7.3M seed from First Round + YC. MicroVM (Firecracker). Purpose-built for AI agents. Blog posts, docs, and Northflank comparison confirm real product.
- **Status**: Active
- **Include in final list**: Yes

#### Runloop
- **Repo/URL**: https://runloop.ai
- **Stars**: N/A (proprietary, SDKs open source)
- **Description**: Enterprise-grade sandbox infrastructure (Devboxes) for AI coding agents at scale.
- **Agent Relevance**: Direct
- **Verification**: Founded by former Google File System tech lead. SOC2-compliant. LangChain DeepAgents integration confirmed.
- **Status**: Active
- **Include in final list**: Yes

#### Cloudflare Sandbox SDK
- **Repo**: https://github.com/cloudflare/sandbox-sdk
- **Stars**: 910 (verified; research said ~2,000 — overestimated)
- **Description**: Secure isolated container environments on Cloudflare's edge for AI agent code execution.
- **Agent Relevance**: Direct
- **Verification**: README confirms "AI code execution" use case. OpenAI Agents example. Launched June 2025 for AI agents. Beta, latest update Feb 2026.
- **Status**: Active (Beta)
- **Include in final list**: Yes

#### Northflank
- **Repo/URL**: https://northflank.com/product/sandboxes
- **Stars**: N/A (proprietary)
- **Description**: Enterprise cloud platform with Kata Containers or gVisor microVM sandboxes, BYOC support.
- **Agent Relevance**: Direct
- **Verification**: Blog posts confirm 2M+ isolated workloads monthly. Comparison articles against Blaxel/E2B confirm market position.
- **Status**: Active
- **Include in final list**: Yes

#### Novita AI Sandbox
- **Repo/URL**: https://novita.ai/sandbox
- **Stars**: N/A (proprietary)
- **Description**: E2B-compatible sandbox runtime with per-second billing and sub-200ms startup.
- **Agent Relevance**: Direct
- **Verification**: E2B-compatible API confirmed. Limited independent verification beyond their own marketing.
- **Status**: Active
- **Include in final list**: Yes (but lower confidence — limited independent evidence)

#### StackBlitz bolt.new
- **Repo**: https://github.com/stackblitz/bolt.new
- **Stars**: 16,200 (verified; research said ~19,000 combined with bolt.diy)
- **Description**: AI-powered web dev agent using WebContainers for in-browser sandboxed full-stack development.
- **Agent Relevance**: Direct
- **Verification**: README confirms "AI-powered web development agent" with full environment control via WebContainers. StackBlitz valued at $700M.
- **Status**: Active
- **Include in final list**: Yes

#### bolt.diy
- **Repo**: https://github.com/stackblitz-labs/bolt.diy
- **Stars**: 19,000 (verified)
- **Description**: Open-source AI coding agent supporting 19+ LLM providers, using WebContainers sandbox.
- **Agent Relevance**: Direct
- **Verification**: Community fork with more stars than original. 19+ LLM providers confirmed. MIT licensed (but WebContainers requires commercial license).
- **Status**: Active
- **Include in final list**: Yes

#### Gitpod / Ona
- **Repo**: https://github.com/gitpod-io/gitpod
- **Stars**: 13,600 (verified)
- **Description**: Cloud dev environment platform rebranded to Ona — "mission control for software engineering agents."
- **Agent Relevance**: Direct
- **Verification**: Confirmed rebrand to Ona. README mentions "Ona Agents." Isolated containerized workspaces per agent.
- **Status**: Active
- **Include in final list**: Yes

#### Coder
- **Repo**: https://github.com/coder/coder
- **Stars**: 12,300 (verified; research said ~9,000)
- **Description**: Self-hosted enterprise platform for AI coding agents in governed cloud dev environments.
- **Agent Relevance**: Direct
- **Verification**: README references "developers and their agents." AGENTS.md file present. Terraform-based infrastructure. v2.29.7 (Feb 2026).
- **Status**: Active
- **Include in final list**: Yes

#### OpenAI Codex
- **Repo**: https://github.com/openai/codex
- **Stars**: 61,900 (verified; research said ~23,000 — massively underestimated)
- **Description**: Cloud-based AI coding agent with secure isolated containers and configurable sandbox modes.
- **Agent Relevance**: Direct
- **Verification**: Confirmed "coding agent from OpenAI." Sandbox with configurable modes. MCP support. v0.105.0 (Feb 2026).
- **Status**: Active
- **Include in final list**: Yes

#### OpenHands (formerly OpenDevin)
- **Repo**: https://github.com/All-Hands-AI/OpenHands
- **Stars**: 68,200 (verified)
- **Description**: Open platform for AI software developers — Docker-based sandboxed execution for code, commands, and browsing.
- **Agent Relevance**: Direct
- **Verification**: Confirmed "AI-driven development" platform. Docker sandbox per session. $18.8M Series A. MIT licensed.
- **Status**: Active
- **Include in final list**: Yes

#### SWE-agent
- **Repo**: https://github.com/SWE-agent/SWE-agent
- **Stars**: 18,600 (verified; research said ~14,200)
- **Description**: Autonomous agent for fixing GitHub issues using LLMs with SWE-ReX sandboxed execution.
- **Agent Relevance**: Direct
- **Verification**: README confirms autonomous tool use with sandbox. State-of-the-art on SWE-bench. MIT licensed.
- **Status**: Active
- **Include in final list**: Yes

#### SWE-ReX
- **Repo**: https://github.com/SWE-agent/SWE-ReX
- **Stars**: 442 (verified; research said ~500)
- **Description**: Runtime interface for sandboxed shell environments for AI agents — powers SWE-agent.
- **Agent Relevance**: Direct
- **Verification**: README: "Sandboxed code execution for AI agents." Powers SWE-agent. v1.4.0.
- **Status**: Active
- **Include in final list**: Yes

#### Composio
- **Repo**: https://github.com/ComposioHQ/composio
- **Stars**: 27,200 (verified; research said ~15,000 — significantly underestimated)
- **Description**: AI agent integration platform with 1000+ toolkits, sandboxed workbench, and managed auth.
- **Agent Relevance**: Direct
- **Verification**: README: "sandboxed workbench." Integrates with OpenAI, Anthropic, LangChain, CrewAI, AutoGen. Very active.
- **Status**: Active
- **Include in final list**: Yes

#### CrewAI
- **Repo**: https://github.com/crewAIInc/crewAI
- **Stars**: 44,600 (verified; research said ~28,000 — significantly underestimated)
- **Description**: Python framework for orchestrating autonomous AI agents with CodeInterpreterTool (Docker/sandbox modes).
- **Agent Relevance**: Direct
- **Verification**: Confirmed "Framework for orchestrating role-playing, autonomous AI agents." CodeInterpreterTool with Docker/sandbox modes.
- **Status**: Active
- **Include in final list**: Yes

#### HuggingFace smolagents
- **Repo**: https://github.com/huggingface/smolagents
- **Stars**: 25,600 (verified; research said ~15,000 — significantly underestimated)
- **Description**: Barebones library for code-writing AI agents with 5 sandbox backends (Blaxel, E2B, Modal, Docker, Pyodide+Deno).
- **Agent Relevance**: Direct
- **Verification**: README confirms sandbox support with 5 providers. CodeAgent writes Python. v1.24.0.
- **Status**: Active
- **Include in final list**: Yes

#### LangChain DeepAgents
- **Repo**: https://github.com/langchain-ai/deepagents
- **Stars**: 9,600 (verified; research said ~3,000 — massively underestimated)
- **Description**: Agent harness built on LangChain/LangGraph with remote sandbox backends (Modal, Runloop, Daytona).
- **Agent Relevance**: Direct
- **Verification**: README: "execute for running commands (with sandboxing)" and "remote sandboxes (Modal, Runloop, Daytona)." Active CLI releases.
- **Status**: Active
- **Include in final list**: Yes

#### Kubernetes Agent Sandbox
- **Repo**: https://github.com/kubernetes-sigs/agent-sandbox
- **Stars**: 1,100 (verified; research said ~2,500 — overestimated)
- **Description**: Kubernetes CRD and controller for managing isolated, stateful sandbox workloads for AI agent runtimes.
- **Agent Relevance**: Direct
- **Verification**: README: "ideal for use cases like AI agent runtimes." SIG Apps subproject. gVisor + Kata support. Warm Pool for sub-second startup.
- **Status**: Active
- **Include in final list**: Yes

#### microsandbox
- **Repo**: https://github.com/zerocore-ai/microsandbox
- **Stars**: 4,800 (verified)
- **Description**: Self-hosted sandbox platform with hardware-level microVM isolation (libkrun) and sub-200ms startup.
- **Agent Relevance**: Direct
- **Verification**: README: "execution backbone of the agentic web." MCP integration. Self-hosted only. libkrun microVM isolation.
- **Status**: Active
- **Include in final list**: Yes

#### AIO Sandbox (agent-infra)
- **Repo**: https://github.com/agent-infra/sandbox
- **Stars**: 2,600 (verified; research said ~3,000)
- **Description**: All-in-one sandbox combining Browser + Shell + File + MCP + VSCode in a single Docker container for AI agents.
- **Agent Relevance**: Direct
- **Verification**: README: "unified, secure execution environment for AI agents." MCP-compatible APIs. v1.0.0.
- **Status**: Active
- **Include in final list**: Yes

#### Arrakis
- **Repo**: https://github.com/abshkbh/arrakis
- **Stars**: 763 (verified; research said ~1,500 — overestimated)
- **Description**: Self-hosted sandboxing with cloud-hypervisor microVM isolation and snapshot/restore backtracking.
- **Agent Relevance**: Direct
- **Verification**: README: "AI agents can generate malicious or buggy code." MicroVM isolation. REST API + Python SDK + MCP server. Backtracking via snapshots.
- **Status**: Active
- **Include in final list**: Yes

#### Rivet Sandbox Agent SDK
- **Repo**: https://github.com/rivet-dev/sandbox-agent
- **Stars**: 938 (verified; research said ~1,000)
- **Description**: Universal HTTP API for controlling coding agents (Claude Code, Codex, OpenCode, Amp) inside sandboxes.
- **Agent Relevance**: Direct
- **Verification**: README: "Run Coding Agents in Sandboxes. Control Them Over HTTP." Supports multiple agents. Static Rust binary. v0.2.1 (Feb 2026).
- **Status**: Active
- **Include in final list**: Yes

#### nono
- **Repo**: https://github.com/lukehinds/nono
- **Stars**: 586 (verified; research listed N/A)
- **Description**: Secure, kernel-enforced sandbox CLI and SDKs for AI agents using Landlock (Linux) and Seatbelt (macOS).
- **Agent Relevance**: Direct
- **Verification**: Purpose-built for AI agents. Blog posts confirm use with Claude Code. Cryptographic audit trail. Apache 2.0. Created by former Red Hat Distinguished Engineer.
- **Status**: Active
- **Include in final list**: Yes

#### Replit Agent
- **Repo/URL**: https://replit.com
- **Stars**: N/A (proprietary)
- **Description**: AI coding agent platform with snapshot engine, versioned databases, and Docker container sandboxes.
- **Agent Relevance**: Direct
- **Verification**: Well-known platform. Snapshot engine for time-travel and rollback confirmed in multiple sources.
- **Status**: Active
- **Include in final list**: Yes

#### Amazon Bedrock AgentCore Code Interpreter
- **Repo/URL**: https://aws.amazon.com/bedrock/agentcore/
- **Stars**: N/A (proprietary AWS service)
- **Description**: Fully managed service for AI agents to securely execute code in isolated sandbox environments on AWS.
- **Agent Relevance**: Direct
- **Verification**: AWS product. Supports Strands, LangChain, LangGraph, CrewAI frameworks.
- **Status**: Active
- **Include in final list**: Yes

#### Morph Cloud
- **Repo**: https://github.com/morph-labs/morphcloud-examples-public
- **Stars**: ~200 (examples repo only)
- **Description**: Cloud infrastructure for AI agents with environment branching and burst scalability.
- **Agent Relevance**: Direct
- **Verification**: YC-backed. Real product but limited open-source footprint. Fast Apply and WarpGrep are companion tools.
- **Status**: Active
- **Include in final list**: Yes (but note minimal OSS presence)

#### Beam Cloud (beta9)
- **Repo**: https://github.com/beam-cloud/beta9
- **Stars**: 1,600 (verified; research listed N/A)
- **Description**: Open-source serverless runtime with sandbox for running LLM-generated code in isolated containers.
- **Agent Relevance**: Direct
- **Verification**: README includes sandbox creation example for LLM-generated code. AGPL-3.0 license. Serverless GPU support.
- **Status**: Active
- **Include in final list**: Yes

#### Google Firebase Studio
- **Repo/URL**: https://firebase.studio
- **Stars**: N/A (proprietary)
- **Description**: Google's agentic cloud IDE with Gemini AI and sandboxed Cloud Workstations environments.
- **Agent Relevance**: Indirect
- **Verification**: Renamed from Project IDX. Cloud Workstations VMs with Nix. Not primarily a sandbox product.
- **Status**: Active
- **Include in final list**: No (IDE, not a sandbox — indirect relevance at best)

#### Aider
- **Repo**: https://github.com/Aider-AI/aider
- **Stars**: 40,900 (verified; research said ~35,000)
- **Description**: AI pair programming tool that can be run in Docker for sandboxed execution.
- **Agent Relevance**: Indirect
- **Verification**: README does NOT mention sandbox, isolation, or code execution. It's an AI coding tool that happens to support Docker deployment. Not a sandbox itself.
- **Status**: Active
- **Include in final list**: No (not a sandbox; it's a coding agent that uses sandboxes)

#### Devon
- **Repo**: https://github.com/entropy-research/Devon
- **Stars**: 3,500 (verified)
- **Description**: Open-source AI pair programmer with multi-file editing.
- **Agent Relevance**: Indirect
- **Verification**: An AI coding agent, not a sandbox solution. Directory-scoped access is not real sandboxing.
- **Status**: Active
- **Include in final list**: No (not a sandbox)

#### AutoGPT
- **Repo**: https://github.com/Significant-Gravitas/AutoGPT
- **Stars**: 182,000 (verified)
- **Description**: Platform for creating and deploying continuous AI agents.
- **Agent Relevance**: Indirect
- **Verification**: NOT a sandbox. Does not mention sandbox, isolation, or code execution security. It's an agent platform.
- **Status**: Active
- **Include in final list**: No (not a sandbox)

#### Gradio
- **Repo**: https://github.com/gradio-app/gradio
- **Stars**: ~37,000 (not re-verified)
- **Description**: Python library for building ML web apps.
- **Agent Relevance**: Indirect
- **Verification**: Not a sandbox. Runs on HuggingFace Spaces which provides isolation, but Gradio itself is not a sandboxing technology.
- **Status**: Active
- **Include in final list**: No (not a sandbox)

#### RunPod
- **Repo/URL**: https://www.runpod.io
- **Stars**: N/A (proprietary)
- **Description**: GPU cloud platform with serverless endpoints.
- **Agent Relevance**: Indirect
- **Verification**: GPU infrastructure, not a sandbox. Instant Agents is an agent deployment feature, not sandbox isolation.
- **Status**: Active
- **Include in final list**: No (GPU cloud, not a sandbox)

#### DevPod
- **Repo**: https://github.com/loft-sh/devpod
- **Stars**: 14,700 (verified; research said ~10,000)
- **Description**: Open-source tool for reproducible dev environments using Dev Containers.
- **Agent Relevance**: Indirect
- **Verification**: General dev environment tool, not specifically for AI agents. Project reportedly on hold.
- **Status**: On hold
- **Include in final list**: No (on hold, indirect relevance)

#### Toolhouse
- **Repo/URL**: https://toolhouse.ai
- **Stars**: N/A (proprietary)
- **Description**: Backend-as-a-Service for AI agents with built-in sandbox, RAG, and memory.
- **Agent Relevance**: Direct
- **Verification**: Limited independent verification. Proprietary with open-source examples. Sandbox is one feature among many.
- **Status**: Active
- **Include in final list**: Yes (but lower confidence)

#### AgentStation
- **Repo/URL**: https://www.agentstation.ai
- **Stars**: N/A (proprietary)
- **Description**: Serverless platform for AI virtual workstations.
- **Agent Relevance**: Direct
- **Verification**: Limited independent verification. Early access only. Novel Zoom meeting participation feature.
- **Status**: Active (early access)
- **Include in final list**: No (too early, no verifiable traction)

---

### Category: Language Runtimes & Code Execution

#### Deno (+ Deno Sandbox cloud product)
- **Repo**: https://github.com/denoland/deno
- **Stars**: 106,000 (verified)
- **Description**: Modern JS/TS runtime with built-in permissions-based security. Deno Sandbox is the cloud product offering Firecracker microVMs for AI agent code execution.
- **Agent Relevance**: Direct
- **Verification**: "Secure defaults" confirmed in README. Deno Sandbox cloud product used by LangChain, Pydantic AI, LMStudio. Granular permission flags.
- **Status**: Active (v2.7.1, Feb 2026)
- **Include in final list**: Yes

#### Bun
- **Repo**: https://github.com/oven-sh/bun
- **Stars**: 87,600 (verified; research said ~78,000)
- **Description**: Fast all-in-one JavaScript runtime. Acquired by Anthropic.
- **Agent Relevance**: Potential (evolving toward Direct)
- **Verification**: README does NOT mention sandbox or permissions. GitHub Issue #25929 proposes permissions model. Anthropic acquisition signals intent but no sandbox feature exists yet.
- **Status**: Active
- **Include in final list**: Yes (notable due to Anthropic acquisition and trajectory)

#### Pyodide
- **Repo**: https://github.com/pyodide/pyodide
- **Stars**: 14,200 (verified)
- **Description**: CPython compiled to WebAssembly for browser and Node.js — foundation for many AI agent sandboxes.
- **Agent Relevance**: Direct
- **Verification**: Foundation for LangChain Sandbox, Cohere Terrarium, mcp-run-python, smolagents. NVIDIA recommends for agentic AI sandboxing. AGENTS.md present. v0.29.3 (Jan 2026).
- **Status**: Active
- **Include in final list**: Yes

#### Pydantic Monty
- **Repo**: https://github.com/pydantic/monty
- **Stars**: 5,700 (verified)
- **Description**: Minimal, secure Python interpreter written in Rust for AI — microsecond startup, no filesystem/network access by default.
- **Agent Relevance**: Direct
- **Verification**: README: "safely run Python code written by an LLM." Designed specifically for AI agents. Successor to mcp-run-python. v0.0.7 (Feb 2026).
- **Status**: Active
- **Include in final list**: Yes

#### quickjs-emscripten
- **Repo**: https://github.com/justjake/quickjs-emscripten
- **Stars**: 1,600 (verified)
- **Description**: Safely execute untrusted JavaScript via QuickJS compiled to WebAssembly.
- **Agent Relevance**: Direct
- **Verification**: Sandboxed execution confirmed. 112k+ weekly npm downloads. Foundation for higher-level sandboxes. No AI-specific features but widely used for sandboxed JS.
- **Status**: Active (maintenance mode)
- **Include in final list**: Yes

#### @sebastianwessel/quickjs
- **Repo**: https://github.com/sebastianwessel/quickjs
- **Stars**: 867 (verified)
- **Description**: TypeScript package for executing JS/TS in a WebAssembly QuickJS sandbox with virtual filesystem.
- **Agent Relevance**: Direct
- **Verification**: WebAssembly sandbox confirmed. Has QuickJS Sandbox MCP server. No explicit AI mentions in README but blog posts detail AI integration.
- **Status**: Active (v3.0.1, Feb 2026)
- **Include in final list**: Yes

#### isolated-vm
- **Repo**: https://github.com/laverdet/isolated-vm
- **Stars**: ~2,700 (not re-verified)
- **Description**: Access V8's Isolate interface from Node.js — true isolate separation with separate heaps.
- **Agent Relevance**: Indirect
- **Verification**: Real V8 isolate separation. Recommended replacement for vm2. Used by Temporal SDK. Not specifically for AI.
- **Status**: Active (maintenance mode)
- **Include in final list**: Yes

#### Piston
- **Repo**: https://github.com/engineer-man/piston
- **Stars**: 2,600 (verified)
- **Description**: High-performance code execution engine using Linux namespaces, chroot, cgroups inside Docker. 70+ languages.
- **Agent Relevance**: Direct
- **Verification**: MCP server available (piston-mcp). Sandboxed execution with 70+ languages. API restricted to non-commercial use as of Feb 2026.
- **Status**: Active (API restrictions)
- **Include in final list**: Yes

#### Judge0
- **Repo**: https://github.com/judge0/judge0
- **Stars**: 4,000 (verified; research said ~3,700)
- **Description**: Robust, scalable online code execution system for humans and AI — 90+ languages with sandboxed execution.
- **Agent Relevance**: Direct
- **Verification**: README: "for humans and AI." AI agents listed as use case. MCP server available. Docker + Isolate sandboxing.
- **Status**: Active
- **Include in final list**: Yes

#### Rhai
- **Repo**: https://github.com/rhaiscript/rhai
- **Stars**: 5,200 (verified)
- **Description**: Embedded scripting language for Rust — sandboxed by default with "Don't Panic" guarantee.
- **Agent Relevance**: Potential
- **Verification**: README: "Sand-boxed" and "Rugged — protected against malicious attacks." No AI agent use. Useful for Rust-based agent frameworks needing embedded scripting.
- **Status**: Active
- **Include in final list**: Yes (sandboxed-by-design scripting for Rust)

#### RustPython
- **Repo**: https://github.com/RustPython/RustPython
- **Stars**: 21,800 (verified)
- **Description**: Python interpreter written in Rust — embeddable, compiles to WebAssembly.
- **Agent Relevance**: Potential
- **Verification**: WebAssembly compilation confirmed. No AI/sandbox mentions. Not production-ready. Control over interpreter capabilities could benefit sandboxing.
- **Status**: Active (in development)
- **Include in final list**: Yes (potential for agent sandboxing via Wasm compilation)

#### RestrictedPython
- **Repo**: https://github.com/zopefoundation/RestrictedPython
- **Stars**: ~628 (not re-verified)
- **Description**: Restricted execution environment for Python — compilation-level restrictions (NOT a full sandbox).
- **Agent Relevance**: Indirect
- **Verification**: Explicitly "not a sandbox." Multiple CVEs for sandbox escape. Must be combined with other measures.
- **Status**: Active
- **Include in final list**: No (explicitly not a sandbox, multiple escape CVEs)

#### Dagger
- **Repo**: https://github.com/dagger/dagger
- **Stars**: 15,500 (verified)
- **Description**: CI/CD automation engine with container-based sandboxed function execution.
- **Agent Relevance**: Indirect
- **Verification**: README: "Tools run in containers, orchestrated by sandboxed functions." Founded by Docker creator. Could be used for agent workflow orchestration.
- **Status**: Active
- **Include in final list**: Yes (indirect but significant — container-based sandboxed function execution)

#### Starlark
- **Repo**: https://github.com/google/starlark-go
- **Stars**: ~2,190 (not re-verified)
- **Description**: Google's deterministic, hermetic configuration language — no filesystem/network/clock access.
- **Agent Relevance**: Potential
- **Verification**: Hermetic execution confirmed. No AI agent use. Could be used for safe agent configuration scripting.
- **Status**: Active
- **Include in final list**: No (config language, no agent relevance demonstrated)

#### Jsonnet / Dhall / CUE
- **Repos**: Various
- **Agent Relevance**: Potential
- **Verification**: Config languages with safety properties. None have demonstrated AI agent use.
- **Include in final list**: No (config languages without agent relevance)

#### Cohere Terrarium
- **Repo**: https://github.com/cohere-ai/cohere-terrarium
- **Stars**: 306 (verified)
- **Description**: Python sandbox for LLM data agents using Pyodide.
- **Agent Relevance**: Direct
- **Verification**: Only 3 total commits. Purpose-built by Cohere for LLM agents but effectively abandoned.
- **Status**: Stale (only 3 commits ever)
- **Include in final list**: No (effectively abandoned, only 3 commits)

#### CodeJail (OpenEdx)
- **Repo**: https://github.com/openedx/codejail
- **Stars**: ~80 (not re-verified)
- **Description**: AppArmor-based sandbox for untrusted code execution in education.
- **Agent Relevance**: Indirect
- **Verification**: Production-tested at edX scale. Not for AI agents. Low stars.
- **Status**: Active
- **Include in final list**: No (education-focused, low stars, not for agents)

#### PyPy Sandbox
- **Repo**: https://github.com/pypy/pypy (sandbox branches)
- **Stars**: N/A (sandbox is a branch)
- **Description**: PyPy sandbox mode — all external calls intercepted by controller process.
- **Agent Relevance**: Potential
- **Verification**: Novel approach but prototype only. Stale.
- **Status**: Stale
- **Include in final list**: No (stale prototype)

#### Compiler Explorer (Godbolt)
- **Repo**: https://github.com/compiler-explorer/compiler-explorer
- **Stars**: ~18,600 (not re-verified)
- **Description**: Interactive compiler explorer — 30+ languages.
- **Agent Relevance**: Indirect
- **Verification**: Not for AI agents. Code compilation/visualization tool.
- **Status**: Active
- **Include in final list**: No (not a sandbox for agents)

#### Wandbox / Glot / Sphere Engine
- **Agent Relevance**: Indirect
- **Include in final list**: No (not specifically for agents, limited relevance)

#### Lune / Lua Sandbox / Luau VM
- **Agent Relevance**: Potential
- **Verification**: All sandboxed by design but none target AI agents.
- **Include in final list**: No (no demonstrated agent relevance)

#### SandboxJS
- **Repo**: https://github.com/nyariv/SandboxJS
- **Stars**: ~193 (not re-verified)
- **Description**: Safe eval for JavaScript with prototype function whitelisting.
- **Agent Relevance**: Potential
- **Verification**: Multiple critical CVEs in 2026. Not reliable for security-sensitive use.
- **Status**: Active (but security issues)
- **Include in final list**: No (multiple CVEs, unreliable)

#### Val Town
- **Repo/URL**: https://www.val.town/
- **Stars**: N/A (SaaS)
- **Description**: Collaborative serverless JS/TS platform.
- **Agent Relevance**: Indirect
- **Verification**: Used via integrations for agent code execution. MCP server available. Not primarily a sandbox.
- **Status**: Active
- **Include in final list**: No (SaaS platform, indirect relevance)

---

### Category: Novel / Research

#### Azazel (eBPF monitoring)
- **Repo**: https://github.com/beelzebub-labs/azazel
- **Stars**: 61 (verified)
- **Description**: eBPF-powered silent observer for containerized runtimes — monitors every syscall, file access, and network connection.
- **Agent Relevance**: Direct
- **Verification**: README: explicitly designed for "Agentic AI monitoring." Zero-overhead eBPF tracing. Per-container attachment.
- **Status**: Active (early-stage)
- **Include in final list**: Yes (novel approach — monitoring/observability complement to sandboxing)

#### Gramine (SGX)
- **Repo**: https://github.com/gramineproject/gramine
- **Stars**: ~1,500 (not re-verified)
- **Description**: Library OS for running Linux apps inside Intel SGX enclaves.
- **Agent Relevance**: Potential
- **Verification**: Used for confidential AI inference. RA-TLS attestation. Relevant for privacy-sensitive agent workloads.
- **Status**: Active
- **Include in final list**: Yes (confidential computing for agents)

#### Occlum (SGX)
- **Repo**: https://github.com/occlum/occlum
- **Stars**: ~1,500 (not re-verified)
- **Description**: Memory-safe library OS for Intel SGX — written in Rust.
- **Agent Relevance**: Potential
- **Verification**: Memory-safe (Rust). 1000x faster enclave startup. Relevant for confidential agent execution.
- **Status**: Active
- **Include in final list**: Yes (confidential computing, Rust-based)

#### awesome-sandbox (community resource)
- **Repo**: https://github.com/restyler/awesome-sandbox
- **Stars**: 352 (verified; research said ~500)
- **Description**: Curated list and analysis of modern code sandboxing solutions for AI.
- **Agent Relevance**: Direct (reference resource)
- **Verification**: Comprehensive comparison of sandbox solutions. AI agent focus. Regularly updated.
- **Status**: Active
- **Include in final list**: Yes (as a community resource/reference)

#### Academic Papers (for reference)
1. **Fault-Tolerant Sandboxing for AI Coding Agents** (Dec 2025) — arxiv.org/abs/2512.12806
2. **AgentBound: Policy enforcement for AI agent execution** (Oct 2025) — arxiv.org/pdf/2510.21236
3. **LLM-in-Sandbox Elicits General Agentic Intelligence** (Jan 2026) — arxiv.org/pdf/2601.16206
4. **Systems Security Foundations for Agentic Computing** (2025) — eprint.iacr.org/2025/2173.pdf
5. **RedCodeAgent: Automated Red-Teaming for Code Agents** — openreview.net

---

### Archived / Superseded (notable for historical context)

#### mcp-run-python (Pydantic)
- **Repo**: https://github.com/pydantic/mcp-run-python
- **Stars**: 189 (verified)
- **Status**: **ARCHIVED** (Jan 30, 2026)
- **Reason**: "No safe way to run Python within pyodide safely with reasonable latency." Superseded by **Monty**.

#### LangChain Sandbox
- **Repo**: https://github.com/langchain-ai/langchain-sandbox
- **Stars**: 234 (verified)
- **Status**: **ARCHIVED** (Jan 14, 2026)
- **Reason**: "No longer maintained. Recommend accessing code execution through sandbox APIs or LLM provider APIs." Superseded by **DeepAgents**.

#### Veracruz
- **Repo**: https://github.com/veracruz-project/veracruz
- **Stars**: 198 (verified)
- **Status**: **ARCHIVED** (Feb 18, 2026)
- **Reason**: Transitioning to Emeritus stage of CCC. Seeking new maintainers.

#### Cohere Terrarium
- **Repo**: https://github.com/cohere-ai/cohere-terrarium
- **Stars**: 306 (verified)
- **Status**: Effectively abandoned (only 3 commits ever, last activity 2023-2024)

---

## Verification Summary

| Metric | Count |
|--------|-------|
| Total entries verified | 90+ |
| Included in final list | 55 |
| Excluded (not sandboxes) | 8 (Aider, Devon, AutoGPT, Gradio, RunPod, Firebase Studio, AgentStation, DevPod) |
| Excluded (low quality/abandoned) | 12 (llm-wasm-sandbox, wasmi, WAVM, Lunatic, Veracruz, Timecraft, RestrictedPython, Cohere Terrarium, CodeJail, SandboxJS, config langs, Lua variants) |
| Excluded (indirect/not relevant) | 8 (Compiler Explorer, Wandbox, Glot, Sphere Engine, Val Town, PyPy Sandbox, Starlark, Jsonnet/Dhall/CUE) |
| Archived/Superseded | 4 (mcp-run-python, langchain-sandbox, Veracruz, Cohere Terrarium) |
| Star counts significantly off | 12 (OpenAI Codex 23K→62K, Composio 15K→27K, CrewAI 28K→45K, smolagents 15K→26K, DeepAgents 3K→10K, CodeSandbox SDK 500→101, K8s Agent Sandbox 2.5K→1.1K, Arrakis 1.5K→763, CF Sandbox SDK 2K→910, Coder 9K→12K, DevPod 10K→15K, Bun 78K→88K) |

### Key Corrections from Research
1. **mcp-run-python is ARCHIVED** — not caught in wasm-research.md
2. **LangChain Sandbox is ARCHIVED** — noted in runtime-research but not platform-research
3. **OpenAI Codex has 62K stars** (not 23K) — most significant undercount
4. **Composio has 27K stars** (not 15K)
5. **CrewAI has 45K stars** (not 28K)
6. **smolagents has 26K stars** (not 15K)
7. **DeepAgents has 10K stars** (not 3K)
8. **CodeSandbox SDK has 101 stars** (not 500) — most significant overcount
9. **Kubernetes Agent Sandbox has 1.1K stars** (not 2.5K)
10. **Arrakis has 763 stars** (not 1.5K)
11. **nono** was missing from research — should be included (586 stars, purpose-built for AI agents)
12. **Beam Cloud (beta9)** repo identified: github.com/beam-cloud/beta9 with 1.6K stars
