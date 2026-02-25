# Commercial & Cloud Agent Sandbox Platforms

Research conducted: 2026-02-25

This document catalogs commercial, cloud-hosted, and purpose-built agent sandbox platforms. This is the richest category for AI agent sandboxing, covering dedicated sandbox-as-a-service platforms, cloud development environments repurposed for agents, AI coding agent frameworks with built-in sandboxing, and related infrastructure.

---

## E2B
- **Repo/URL**: https://github.com/e2b-dev/E2B / https://e2b.dev
- **Stars**: ~10,900
- **Open Source**: Yes (Apache-2.0)
- **Description**: Open-source cloud runtime for AI agents providing secure isolated sandboxes powered by Firecracker microVMs.
- **Agent Relevance**: Direct
- **Evidence**: Purpose-built for AI agent code execution. Used by Manus, Hugging Face, and many agent frameworks. SDKs for Python and JS/TS integrate with LangChain, AutoGen, CrewAI. Code Interpreter SDK for running AI-generated code. Sandboxes start in ~80ms.
- **Key Features**: Firecracker microVM isolation, 80ms startup, Code Interpreter SDK, E2B Desktop (GUI sandbox for computer use), custom sandbox templates, Python & JS SDKs, 100% open source including infrastructure.
- **Status**: Active (very active development, latest release Nov 2025)
- **Pricing**: Free tier available, pay-per-use for production

---

## Modal Sandboxes
- **Repo/URL**: https://modal.com/products/sandboxes / https://modal.com
- **Stars**: N/A (proprietary platform, open-source client library)
- **Open Source**: No (proprietary platform)
- **Description**: Python-first cloud platform with purpose-built sandboxes for safely running untrusted AI-generated code at scale.
- **Agent Relevance**: Direct
- **Evidence**: Sandboxes are the Modal primitive for safely running untrusted code from LLMs. Used by Ramp (writes 50%+ of merged PRs), Quora/Poe, Codegen, Relevance AI. Supports SWE-bench evaluations natively. Scales to 50,000+ concurrent sessions.
- **Key Features**: gVisor container isolation, sub-second startup, granular network egress policies, native observability, pay-per-CPU-cycle billing, Python SDK for defining custom images, instant scaling to 50K+ sessions.
- **Status**: Active (GA as of 2025)
- **Pricing**: Pay-per-use (per CPU cycle)

---

## Daytona
- **Repo/URL**: https://github.com/daytonaio/daytona / https://www.daytona.io
- **Stars**: ~60,100
- **Open Source**: Yes (Apache-2.0)
- **Description**: Secure, elastic infrastructure for running AI-generated code with 90ms environment creation and enterprise-grade security.
- **Agent Relevance**: Direct
- **Evidence**: Pivoted in February 2025 from dev environments to AI agent infrastructure. Agent-first design with programmatic APIs. Official LangChain integration. SDKs for Python, TypeScript, and Ruby. Sub-90ms sandbox creation—fastest in market.
- **Key Features**: 90ms cold start, stateful sandboxes, declarative builder, snapshots for pre-built environments, LSP support for code intelligence, customer-managed compute (your cloud or on-prem), MCP server support, Git operations built-in.
- **Status**: Active (extremely popular, grew from 14K to 60K stars in 2025)
- **Pricing**: Open source self-hosted + managed cloud offering

---

## CodeSandbox SDK / Together Code Sandbox
- **Repo/URL**: https://github.com/codesandbox/codesandbox-sdk / https://codesandbox.io/sdk
- **Stars**: ~500 (SDK repo)
- **Open Source**: Partial (SDK is open source, infrastructure is proprietary)
- **Description**: Programmatic API to spin up microVM-based development environments for AI agent code execution, now part of Together AI.
- **Agent Relevance**: Direct
- **Evidence**: SDK designed for AI agent runtime environments. Acquired by Together AI. MicroVM infrastructure (Pitcher) enables snapshot/restore and cloning within seconds. Integrated into Together AI platform as Together Code Sandbox.
- **Key Features**: MicroVM infrastructure, VM snapshot/restore & cloning in seconds, Docker-based custom snapshots, 500ms start from snapshot (2.7s cold), hot-swappable VM sizes (2-64 vCPU), git-versioned filesystem, data centers in US/APAC/EMEA.
- **Status**: Active (under Together AI umbrella)
- **Pricing**: $0.0446/hr/vCPU + $0.0149/hr/GB RAM ($0.01486/credit)

---

## Fly.io Machines / Sprites.dev
- **Repo/URL**: https://fly.io / https://sprites.dev
- **Stars**: N/A (proprietary)
- **Open Source**: No (proprietary platform, open-source local version planned)
- **Description**: Hardware-isolated Firecracker microVMs with Sprites providing persistent, stateful sandboxes purpose-built for AI coding agents.
- **Agent Relevance**: Direct
- **Evidence**: Fly Machines used as fast-booting sandboxes for AI-generated code. Sprites (launched Jan 2026) are persistent VMs specifically for AI agents like Claude Code, with checkpoint/restore in ~300ms. Pre-installed Claude, Gemini, Codex agents.
- **Key Features**: Firecracker microVM isolation, persistent storage (ext4), checkpoint/restore (~300ms), pre-installed AI coding agents, unique URL per Sprite, global edge regions, JuiceFS-like storage architecture.
- **Status**: Active (Sprites launched Jan 2026)
- **Pricing**: $0.07/CPU-hour, $0.04375/GB-hour memory (no charge when idle)

---

## Blaxel
- **Repo/URL**: https://blaxel.ai
- **Stars**: N/A (proprietary)
- **Open Source**: No (proprietary)
- **Description**: Perpetual sandbox platform that keeps infinite secure sandboxes on automatic standby with co-hosted agents for near-instant (<25ms) resume latency.
- **Agent Relevance**: Direct
- **Evidence**: YC Spring 2025 batch. Purpose-built for AI agents—sandboxes scale to zero after 1s of inactivity and resume in <25ms with full memory state. Agent-native MCP integration. Used by SpawnLabs and Mendral (by Docker founders).
- **Key Features**: <25ms resume from standby, microVM isolation, co-location of agent APIs/MCP servers alongside sandboxes, SOC2/HIPAA compliance, framework-agnostic (LangGraph, CrewAI), zero compute cost when idle.
- **Status**: Active ($7.3M seed from First Round, YC)
- **Pricing**: Pay-per-use, reduced infra costs by 80% for customers

---

## Runloop
- **Repo/URL**: https://runloop.ai / https://github.com/runloopai
- **Stars**: N/A (proprietary platform, open-source SDKs)
- **Open Source**: No (proprietary, SDKs open source)
- **Description**: Enterprise-grade sandbox infrastructure (Devboxes) for building, testing, and deploying AI coding agents at scale.
- **Agent Relevance**: Direct
- **Evidence**: Founded by former tech lead of Google File System and founding engineer at Google Wallet. SOC2-compliant. Supports 10,000+ parallel instances. Integrated with LangChain DeepAgents. Blueprints and Snapshots for environment management.
- **Key Features**: VM-based Devboxes, few-second startup, snapshot/suspend/resume, customizable blueprints, network egress policies, dynamic CPU/memory scaling, SWE-bench integration, Python & TypeScript SDKs, SOC2 certified.
- **Status**: Active
- **Pricing**: Pay-per-use, hosted cloud or self-hosted in your AWS/GCP

---

## Cloudflare Sandbox SDK
- **Repo/URL**: https://github.com/cloudflare/sandbox-sdk / https://developers.cloudflare.com/sandbox/
- **Stars**: ~2,000 (estimated)
- **Open Source**: Yes (SDK is open source)
- **Description**: Secure isolated container environments for running untrusted code on Cloudflare's edge network, with built-in AI agent support.
- **Agent Relevance**: Direct
- **Evidence**: Launched June 2025 specifically for AI agents executing code. Native Workers AI integration. Examples include running Claude Code and OpenCode headless in sandboxes. Code interpreter with streaming support.
- **Key Features**: Edge network deployment, millisecond startup, streaming stdout/stderr, persistent code interpreters (Python/JS), Git operations, file system access, public URL exposure for services, AI code executor tutorial.
- **Status**: Active (Beta, major update Aug 2025)
- **Pricing**: Integrated with Cloudflare Workers pricing

---

## Northflank
- **Repo/URL**: https://northflank.com/product/sandboxes
- **Stars**: N/A (proprietary)
- **Open Source**: No (proprietary)
- **Description**: Enterprise cloud platform with secure microVM sandboxes, offering choice of Kata Containers or gVisor isolation, with BYOC support.
- **Agent Relevance**: Direct
- **Evidence**: Operating sandbox infrastructure since 2019, processing 2M+ isolated workloads monthly. Companies like Writer, Sentry run multi-tenant AI workloads. Only platform offering both Kata Containers and gVisor isolation choices.
- **Key Features**: Kata Containers + gVisor isolation options, BYOC (AWS/GCP/Azure/bare-metal), unlimited session duration, any OCI container image, GPU support (H100s at $2.74/hr), free sandbox tier, sub-second microVM boot.
- **Status**: Active
- **Pricing**: $0.01667/vCPU-hour (up to 65% cheaper than competitors), free tier available

---

## Novita AI Sandbox
- **Repo/URL**: https://novita.ai/sandbox
- **Stars**: N/A (proprietary)
- **Open Source**: No (proprietary)
- **Description**: Secure, fast, and programmable runtime for AI agent execution with E2B-compatible API and per-second billing.
- **Agent Relevance**: Direct
- **Evidence**: E2B-compatible SDK for easy migration. Sub-200ms startup. Serves companies like Quora and Hugging Face. Agent Runtime supports LangGraph, AutoGen, and custom implementations.
- **Key Features**: <200ms startup, E2B-compatible API, massive concurrency (thousands of parallel instances), VNC GUI support, multi-language (Python/JS/C++), per-second billing, Agent Runtime for serverless agent deployment.
- **Status**: Active
- **Pricing**: Per-second billing, no subscription fees

---

## StackBlitz WebContainers / Bolt.new
- **Repo/URL**: https://github.com/stackblitz/bolt.new / https://webcontainers.io
- **Stars**: ~19,000 (bolt.new/bolt.diy combined)
- **Open Source**: Partial (bolt.new source available, WebContainers proprietary)
- **Description**: Browser-based Node.js runtime using WebAssembly that powers AI coding agents like Bolt.new with zero-latency, client-side sandboxing.
- **Agent Relevance**: Direct
- **Evidence**: Bolt.new is an AI coding agent giving LLMs complete control over filesystem, node server, package manager, terminal, and browser console—all running in-browser via WebContainers. $700M valuation talks in Jan 2025.
- **Key Features**: Zero-latency (runs entirely in browser), no server infrastructure needed, offline capable, full Node.js environment in WebAssembly, supports npm/pnpm/yarn, 10x faster than local for package management.
- **Status**: Active ($113.4M total funding)
- **Pricing**: Bolt.new has free and paid tiers, WebContainers requires commercial license for production

---

## bolt.diy
- **Repo/URL**: https://github.com/stackblitz-labs/bolt.diy
- **Stars**: ~19,000
- **Open Source**: Yes (MIT, but WebContainers API requires commercial license for production)
- **Description**: Open-source AI coding agent supporting 19+ LLM providers, using WebContainers for in-browser sandboxed full-stack development.
- **Agent Relevance**: Direct
- **Evidence**: Community-driven open-source alternative to Bolt.new. Supports OpenAI, Anthropic, Ollama, and 16+ other LLM providers. Full sandbox environment with live preview, file system access, and terminal integration.
- **Key Features**: 19+ LLM provider support, MCP server integration, in-browser WebContainer sandbox, live preview, image/file attachment to prompts, Docker deployment support.
- **Status**: Active (very active community)
- **Pricing**: Free (open source), WebContainers commercial license needed for production

---

## Gitpod / Ona
- **Repo/URL**: https://github.com/gitpod-io/gitpod / https://www.gitpod.io
- **Stars**: ~13,000
- **Open Source**: Partial (core open source, enterprise features proprietary)
- **Description**: Cloud development environment platform rebranded to Ona (Sep 2025), pivoted to "mission control for AI software engineering agents."
- **Agent Relevance**: Direct
- **Evidence**: Rebranded to Ona as an AI agent orchestration platform. Each agent gets isolated dev environment. SDK for building/deploying agents at scale. Full VS Code in browser with sandboxed high-autonomy agent execution.
- **Key Features**: Isolated containerized workspaces per agent, SDK for agent orchestration, VPC deployment option, agent execution logging and visibility, pre-configured environments, VS Code integration.
- **Status**: Active (rebranded to Ona Sep 2025)
- **Pricing**: Free tier, team and enterprise plans

---

## Coder
- **Repo/URL**: https://github.com/coder/coder / https://coder.com
- **Stars**: ~9,000
- **Open Source**: Yes (AGPL-3.0)
- **Description**: Self-hosted enterprise platform for running AI coding agents in governed cloud development environments with full infrastructure-as-code control.
- **Agent Relevance**: Direct
- **Evidence**: Purpose-built AI agent integration with Agent Boundaries for process-level safeguards. Supports GPT-Code, OpenDevin, SWE-Agent in Coder workspaces. Custom agents via MCP servers. Coder registry for agent contributions.
- **Key Features**: Terraform-based infrastructure-as-code, Agent Boundaries (process-level safeguards), self-hosted on any cloud or air-gapped, rapid workspace provisioning, custom agent support via MCP, enterprise governance and audit.
- **Status**: Active
- **Pricing**: Free (open source), enterprise plans available

---

## Google Firebase Studio (formerly Project IDX)
- **Repo/URL**: https://firebase.studio / https://idx.dev
- **Stars**: N/A (proprietary)
- **Open Source**: No (proprietary)
- **Description**: Google's agentic cloud-based IDE with AI-powered development, multimodal prompting, and full-stack sandboxed environments powered by Cloud Workstations.
- **Agent Relevance**: Indirect
- **Evidence**: Renamed from Project IDX to Firebase Studio (Apr 2025). Uses Cloud Workstations VMs with Nix for reproducible sandboxed environments. App Prototyping agent generates full-stack applications. Gemini AI integration.
- **Key Features**: Cloud Workstations VM-based sandboxes, Nix reproducibility, embedded Android/iOS simulators, Gemini AI integration, App Prototyping agent, multimodal prompting (text, images, drawing).
- **Status**: Active (evolved into Firebase Studio)
- **Pricing**: Free tier, integrated with Google Cloud billing

---

## OpenAI Codex
- **Repo/URL**: https://github.com/openai/codex / https://openai.com/codex
- **Stars**: ~23,000 (CLI tool)
- **Open Source**: Partial (CLI open source, cloud platform proprietary)
- **Description**: Cloud-based AI software engineering agent with secure isolated containers, supporting parallel task execution and automated PR creation.
- **Agent Relevance**: Direct
- **Evidence**: Operates in secure, isolated cloud containers with internet access disabled by default. OS-enforced local sandbox. Multi-agent support via MCP. GPT-5-Codex can work independently for 7+ hours. Skills and Automations for CI/CD.
- **Key Features**: Isolated cloud containers, configurable sandbox modes, approval policies, parallel task execution, Skills and Automations, MCP server support, optional internet access with domain-level granularity.
- **Status**: Active (launched Apr 2025, major upgrades ongoing)
- **Pricing**: Included with ChatGPT Plus/Pro/Enterprise subscriptions

---

## Replit Agent
- **Repo/URL**: https://replit.com
- **Stars**: N/A (proprietary)
- **Open Source**: No (proprietary)
- **Description**: AI coding agent platform with snapshot engine for instant filesystem forks, versioned databases, and isolated sandboxes enabling reversible development.
- **Agent Relevance**: Direct
- **Evidence**: Agent 3 works autonomously for 200+ minutes. Each user gets their own Docker container sandbox with Postgres. Snapshot engine provides time-travel and rollback capabilities. Self-testing via Playwright with 90% autonomy success rate.
- **Key Features**: Snapshot engine (instant filesystem forks), versioned databases, Docker container isolation per user, Playwright self-testing loop, 200+ minute autonomous sessions, custom MCP server support, code execution API.
- **Status**: Active (Agent v2 Feb 2025, Agent 3 Sep 2025)
- **Pricing**: Free tier, paid plans for more compute

---

## OpenHands (formerly OpenDevin)
- **Repo/URL**: https://github.com/OpenHands/OpenHands / https://openhands.dev
- **Stars**: ~68,000
- **Open Source**: Yes (MIT)
- **Description**: Open platform for AI software developers as generalist agents, with Docker-based sandboxed execution for code, commands, and web browsing.
- **Agent Relevance**: Direct
- **Evidence**: Each task session spins up securely isolated Docker container sandbox. Agents execute arbitrary bash commands and Python code. Supports multi-agent delegation. $18.8M Series A (Nov 2025). 65K+ GitHub stars.
- **Key Features**: Docker sandbox per session, Jupyter kernel integration, browser automation (BrowserGym), multi-agent delegation, composable Python SDK, evaluation benchmarks (SWE-Bench), MIT licensed.
- **Status**: Active (very active, $18.8M funding)
- **Pricing**: Free (open source), cloud offering planned

---

## SWE-agent
- **Repo/URL**: https://github.com/SWE-agent/SWE-agent
- **Stars**: ~14,200
- **Open Source**: Yes (MIT)
- **Description**: Autonomous agent that takes GitHub issues and fixes them using LLMs, with SWE-ReX providing sandboxed code execution locally or in the cloud.
- **Agent Relevance**: Direct
- **Evidence**: State of the art on SWE-bench among open-source projects. SWE-ReX provides sandboxed code execution infrastructure. Published at NeurIPS 2024. Built by Princeton and Stanford researchers.
- **Key Features**: Autonomous GitHub issue resolution, SWE-ReX sandbox execution layer (Docker/AWS), massively parallel execution, interactive CLI tools support (ipython, gdb), configurable and hackable by design.
- **Status**: Active
- **Pricing**: Free (open source)

---

## SWE-ReX
- **Repo/URL**: https://github.com/SWE-agent/SWE-ReX
- **Stars**: ~500
- **Open Source**: Yes (MIT)
- **Description**: Runtime interface for sandboxed shell environments, enabling AI agents to execute commands locally or remotely in Docker/cloud environments.
- **Agent Relevance**: Direct
- **Evidence**: Purpose-built sandbox execution layer for AI agents. Powers SWE-agent. Supports running commands in Docker containers, AWS, and local environments. Massively parallel execution support.
- **Key Features**: Sandboxed shell sessions, recognizes command completion and extracts output/exit codes, interactive CLI support, multiple parallel shell sessions, local or cloud execution.
- **Status**: Active
- **Pricing**: Free (open source)

---

## Aider
- **Repo/URL**: https://github.com/Aider-AI/aider
- **Stars**: ~35,000+
- **Open Source**: Yes (Apache-2.0)
- **Description**: AI pair programming tool in the terminal that maps entire codebases and works with most LLMs, with Docker-based sandboxed execution option.
- **Agent Relevance**: Indirect
- **Evidence**: Not a sandbox platform itself, but a major AI coding agent that can be run in Docker containers for sandboxed execution. Works with Claude, GPT-4o, DeepSeek, local models via Ollama. Auto-commits changes with sensible messages.
- **Key Features**: Whole-codebase mapping, multi-language support, auto-commit, lint/test integration, Docker deployment for sandboxing, local model support via Ollama.
- **Status**: Active (very popular)
- **Pricing**: Free (open source)

---

## Devon
- **Repo/URL**: https://github.com/entropy-research/Devon
- **Stars**: ~3,000
- **Open Source**: Yes
- **Description**: Open-source AI pair programmer with multi-file editing, code exploration, test writing, and bug fix capabilities.
- **Agent Relevance**: Indirect
- **Evidence**: One of many "Devin alternatives." Agent is constrained to the directory it's started from. Uses Anthropic as AI backbone. Supports Claude, GPT-4o, and local models.
- **Key Features**: Multi-file editing, code exploration, test writing, directory-scoped access control, multiple LLM support including local models.
- **Status**: Active (community-driven)
- **Pricing**: Free (open source)

---

## Composio
- **Repo/URL**: https://github.com/ComposioHQ/composio / https://composio.dev
- **Stars**: ~15,000
- **Open Source**: Yes (open core)
- **Description**: AI agent integration platform with 1000+ toolkits, sandboxed workbench, managed authentication, and support for 25+ agentic frameworks.
- **Agent Relevance**: Direct
- **Evidence**: Provides sandboxed Docker execution environments for AI agents. 850+ pre-built connectors. Used to build Devin-like SWE agents. Integrates with LangChain, AutoGen, CrewAI. MCP server (Rube) connects to 500+ apps.
- **Key Features**: 1000+ toolkits, sandboxed Docker execution, managed OAuth 2.0/API key auth, observability dashboard, 25+ framework integrations, MCP server for 500+ apps.
- **Status**: Active
- **Pricing**: Free tier, paid plans for production

---

## Toolhouse
- **Repo/URL**: https://toolhouse.ai / https://github.com/toolhouseai
- **Stars**: N/A (proprietary, open-source examples)
- **Open Source**: No (proprietary platform)
- **Description**: Backend-as-a-Service for building, running, and managing AI agents with built-in sandbox, RAG, memory, and MCP integration.
- **Agent Relevance**: Direct
- **Evidence**: Each agent gets a dedicated sandbox environment. Built-in MCP server provides RAG, memory, code execution, browser use. SDK integrates with Vercel AI and LlamaIndex.
- **Key Features**: Dedicated sandbox per agent, built-in RAG/memory/code execution, Agent Studio (no-code builder), MCP server integration, Vercel AI/LlamaIndex SDK support.
- **Status**: Active
- **Pricing**: Free (Sandbox: 50 runs/mo), Pro ($10/mo: 1000 runs), Enterprise (custom)

---

## AgentStation
- **Repo/URL**: https://www.agentstation.ai
- **Stars**: N/A (proprietary)
- **Open Source**: No (proprietary)
- **Description**: Serverless platform for AI virtual workstations, enabling agents to use browsers, join Zoom meetings, execute code, and complete web tasks.
- **Agent Relevance**: Direct
- **Evidence**: API for agents to get work done using virtual workstations. Agents can browse the web, write/run code (Python/Go/Node.js), join and participate in Zoom meetings, crawl webpages, and extract data.
- **Key Features**: Virtual workstation API, browser automation, Zoom meeting participation (screen share, speak), multi-language code execution, webpage crawling, $50 free credits for early access.
- **Status**: Active (early access)
- **Pricing**: Credit-based, $50 free for early access

---

## AutoGPT Platform
- **Repo/URL**: https://github.com/Significant-Gravitas/AutoGPT
- **Stars**: ~174,000
- **Open Source**: Partial (MIT for most, Polyform Shield for platform)
- **Description**: Platform for creating, deploying, and managing continuous AI agents with low-code agent builder, workflow management, and marketplace.
- **Agent Relevance**: Indirect
- **Evidence**: Not primarily a sandbox, but provides agent infrastructure where agents run as automated workflows on the AutoGPT Server. Agents operate continuously and can be triggered by external sources.
- **Key Features**: Low-code Agent Builder, workflow block system, continuous agent execution, marketplace for pre-built agents, server-based deployment.
- **Status**: Active
- **Pricing**: Free (open source), hosted platform planned

---

## CrewAI
- **Repo/URL**: https://github.com/crewAIInc/crewAI / https://www.crewai.com
- **Stars**: ~28,000
- **Open Source**: Yes
- **Description**: Python framework for orchestrating autonomous AI agents with built-in CodeInterpreterTool supporting Docker, sandbox, and unsafe execution modes.
- **Agent Relevance**: Direct
- **Evidence**: CodeInterpreterTool enables agents to execute Python in Docker containers (primary), restricted sandbox (fallback), or unsafe mode. Integrates with Amazon Bedrock AgentCore, E2B, and Bearly Code Interpreter for sandbox execution.
- **Key Features**: Multi-agent orchestration, CodeInterpreterTool with Docker/sandbox modes, role-playing agent design, support for external sandbox providers (E2B, Bedrock).
- **Status**: Active
- **Pricing**: Free (open source), enterprise plans

---

## LangChain Sandbox / DeepAgents
- **Repo/URL**: https://github.com/langchain-ai/langchain-sandbox / https://github.com/langchain-ai/deepagents
- **Stars**: ~1,000 (sandbox), ~3,000 (deepagents)
- **Open Source**: Yes (MIT)
- **Description**: Sandbox backends for LangChain's DeepAgents framework, supporting remote execution via Modal, Runloop, Daytona, and more.
- **Agent Relevance**: Direct
- **Evidence**: DeepAgents framework provides filesystem tools and execute tool for running shell commands in remote sandboxes. Supports "Agent in Sandbox" and "Sandbox as Tool" patterns. Integrates with Modal, Runloop, Daytona.
- **Key Features**: Remote sandbox backends (Modal, Runloop, Daytona), filesystem tools (ls, read/write, edit, glob, grep) + execute tool, agent-in-sandbox and sandbox-as-tool patterns. Note: original langchain-sandbox (Pyodide) is deprecated.
- **Status**: Active (DeepAgents approach is current)
- **Pricing**: Free (open source), sandbox provider costs apply

---

## HuggingFace smolagents
- **Repo/URL**: https://github.com/huggingface/smolagents
- **Stars**: ~15,000
- **Open Source**: Yes (Apache-2.0)
- **Description**: Barebones library for code-writing AI agents with sandboxed execution via Blaxel, E2B, Modal, Docker, or Pyodide+Deno.
- **Agent Relevance**: Direct
- **Evidence**: CodeAgent writes actions as code (30% fewer steps than JSON tool-calling). Supports 5 sandbox providers for secure execution. Can be deployed as HuggingFace Spaces. Framework-agnostic model support.
- **Key Features**: CodeAgent (code-based actions), 5 sandbox backends (Blaxel, E2B, Modal, Docker, Pyodide), ToolCallingAgent (JSON), model-agnostic, HuggingFace Hub integration.
- **Status**: Active
- **Pricing**: Free (open source)

---

## Kubernetes Agent Sandbox
- **Repo/URL**: https://github.com/kubernetes-sigs/agent-sandbox / https://agent-sandbox.sigs.k8s.io
- **Stars**: ~2,500
- **Open Source**: Yes (Apache-2.0)
- **Description**: Kubernetes CRD and controller for managing isolated, stateful sandbox workloads designed for AI agent runtimes.
- **Agent Relevance**: Direct
- **Evidence**: Launched as formal subproject of Kubernetes SIG Apps. Introduced at KubeCon NA 2025. Google-backed. Built on gVisor with Kata Containers support. Warm Pool for sub-second latency (90% improvement over cold starts). Python SDK.
- **Key Features**: Sandbox CRD, SandboxTemplate, SandboxClaim APIs, gVisor + Kata Containers isolation, Warm Pool for sub-second startup, deep hibernation with auto-resume, Python SDK, integrates with ADK/LangChain.
- **Status**: Active (CNCF project)
- **Pricing**: Free (open source), runs on any Kubernetes cluster

---

## microsandbox
- **Repo/URL**: https://github.com/zerocore-ai/microsandbox
- **Stars**: ~4,700
- **Open Source**: Yes (Apache-2.0)
- **Description**: Self-hosted sandbox platform combining hardware-level microVM isolation (libkrun) with sub-200ms startup for AI agent code execution.
- **Agent Relevance**: Direct
- **Evidence**: Purpose-built for AI agents with MCP integration. Self-hosted only—no SaaS dependency, data never leaves your network. Hardware-level VM isolation via libkrun (KVM). OCI-compatible (runs standard container images).
- **Key Features**: libkrun microVM isolation, <200ms startup, OCI compatible, self-hosted only, persistent sandbox state (./menv directory), MCP integration, no SaaS dependency.
- **Status**: Active (initial release May 2025)
- **Pricing**: Free (open source, self-hosted)

---

## AIO Sandbox (agent-infra)
- **Repo/URL**: https://github.com/agent-infra/sandbox / https://sandbox.agent-infra.com
- **Stars**: ~3,000
- **Open Source**: Yes (Apache-2.0)
- **Description**: All-in-one sandbox combining Browser, Shell, File, MCP, and VSCode Server in a single Docker container for AI agents.
- **Agent Relevance**: Direct
- **Evidence**: Purpose-built for AI agents. Unified file system across browser/terminal/VSCode. Pre-configured MCP Server. Evaluation framework for tool optimization. Python, TypeScript, and Go SDKs.
- **Key Features**: All-in-one (Browser + Shell + File + MCP + VSCode + Jupyter), unified filesystem, VNC browser, MCP-compatible APIs, Docker/K8s deployment, evaluation framework, 4-8s startup.
- **Status**: Active (v1.0.0)
- **Pricing**: Free (open source)

---

## Arrakis
- **Repo/URL**: https://github.com/abshkbh/arrakis
- **Stars**: ~1,500
- **Open Source**: Yes (AGPL-3.0)
- **Description**: Self-hosted sandboxing solution for AI agent code execution with out-of-the-box backtracking (snapshot/restore) and cloud-hypervisor microVM isolation.
- **Agent Relevance**: Direct
- **Evidence**: Created by former Replit/Google/Microsoft engineer. Each sandbox runs in a cloud-hypervisor microVM. REST API, Python SDK, and MCP server. Automatic port forwarding for GUI/browser access. Backtracking via snapshot-and-restore.
- **Key Features**: cloud-hypervisor microVM isolation, snapshot/restore backtracking, REST API + Python SDK + MCP server, automatic port forwarding, VNC for GUI/computer use, Dockerfile-based rootfs customization, overlayfs protection.
- **Status**: Active
- **Pricing**: Free (open source, self-hosted, Linux only)

---

## Rivet Sandbox Agent SDK
- **Repo/URL**: https://github.com/rivet-dev/sandbox-agent / https://sandboxagent.dev
- **Stars**: ~1,000
- **Open Source**: Yes (Apache-2.0)
- **Description**: Universal HTTP API for controlling coding agents (Claude Code, Codex, OpenCode, Amp) inside sandboxes, with persistent session schema.
- **Agent Relevance**: Direct
- **Evidence**: Addresses agent API fragmentation—one HTTP API to control multiple coding agents. ~15MB static Rust binary runs inside any sandbox (E2B, Daytona, Vercel, Docker). Universal session schema for audit and replay.
- **Key Features**: Universal agent API (Claude Code, Codex, OpenCode, Amp, Pi), single static Rust binary (~15MB), universal session schema, on-demand agent install, works in E2B/Daytona/Vercel/Docker sandboxes, Rivet Actors integration.
- **Status**: Active (launched Jan 2026)
- **Pricing**: Free (open source)

---

## Morph Cloud
- **Repo/URL**: https://github.com/morph-labs/morphcloud-examples-public / https://www.morph.so
- **Stars**: ~200 (examples repo)
- **Open Source**: Partial (examples open source, platform proprietary)
- **Description**: Cloud infrastructure for AI agents with instant environment branching and burst scalability, plus Morph LLM tools (Fast Apply, WarpGrep).
- **Agent Relevance**: Direct
- **Evidence**: Morph Cloud provides sandbox infrastructure for AI agents. Morph LLM offers Fast Apply (98% success rate, 10,500+ tok/s) and WarpGrep (RL-trained retrieval) as agent tools. Used in AI coding pipelines.
- **Key Features**: Environment branching, Jupyter environments, browser automation (CDP), Fast Apply for multi-file edits, WarpGrep for code retrieval, burst scalability.
- **Status**: Active (YC-backed)
- **Pricing**: Usage-based

---

## RunPod
- **Repo/URL**: https://www.runpod.io
- **Stars**: N/A (proprietary)
- **Open Source**: No (proprietary)
- **Description**: GPU cloud platform with serverless endpoints and Instant Agents for deploying AI agents without DevOps, with per-second billing.
- **Agent Relevance**: Indirect
- **Evidence**: Not primarily a sandbox, but provides GPU infrastructure where AI agents can run in containerized environments. Instant Agents enable deploying agents directly to GPU cloud. FlashBoot cold starts <200ms. A4000 to H100 GPUs.
- **Key Features**: Serverless GPU endpoints, Instant Agents deployment, FlashBoot cold starts (<200ms), per-second billing, automatic scaling from zero, A4000-H100 GPU options.
- **Status**: Active
- **Pricing**: Per-second GPU billing, varies by GPU type

---

## DevPod
- **Repo/URL**: https://github.com/loft-sh/devpod / https://devpod.sh
- **Stars**: ~10,000
- **Open Source**: Yes (MPL-2.0)
- **Description**: Open-source tool for creating reproducible development environments using Dev Containers, supporting Docker, Podman, Kubernetes, and cloud providers.
- **Agent Relevance**: Indirect
- **Evidence**: Creates isolated containerized workspaces where Claude Code and other agents can run (host > vm > container > claude). Good isolation from host. Multiple provider backends. Note: project put on hold indefinitely, but forks continue.
- **Key Features**: Dev Container spec support, multiple provider backends (Docker, Podman, K8s, SSH, cloud), VS Code integration, client-only architecture, reproducible environments.
- **Status**: On hold (forks active)
- **Pricing**: Free (open source)

---

## Gradio (via HuggingFace)
- **Repo/URL**: https://github.com/gradio-app/gradio
- **Stars**: ~37,000
- **Open Source**: Yes (Apache-2.0)
- **Description**: Python library for building ML web apps with sandboxed code execution through HuggingFace Spaces deployment.
- **Agent Relevance**: Indirect
- **Evidence**: Not a sandbox itself, but Gradio apps deployed on HuggingFace Spaces run in isolated containerized environments. Used to build AI agent demos and interfaces. Sandboxed execution is provided by the Spaces infrastructure.
- **Key Features**: Easy ML web app creation, HuggingFace Spaces deployment (Docker-based isolation), interactive demos, file upload/download, real-time streaming.
- **Status**: Active
- **Pricing**: Free (open source), HuggingFace Spaces has free and paid tiers

---

## Amazon Bedrock AgentCore Code Interpreter
- **Repo/URL**: https://aws.amazon.com/bedrock/agentcore/
- **Stars**: N/A (proprietary AWS service)
- **Open Source**: No (proprietary)
- **Description**: Fully managed service enabling AI agents to securely execute code in isolated sandbox environments on AWS.
- **Agent Relevance**: Direct
- **Evidence**: Native support for Strands, LangChain, LangGraph, and CrewAI frameworks. Provides isolated sandbox execution environments for code interpretation as part of the broader Bedrock agent infrastructure.
- **Key Features**: Fully managed, framework integrations (Strands, LangChain, LangGraph, CrewAI), isolated sandbox environments, integrated with AWS Bedrock.
- **Status**: Active
- **Pricing**: AWS pay-per-use

---

## Beam (E2B Alternative)
- **Repo/URL**: https://www.beam.cloud
- **Stars**: N/A
- **Open Source**: Partial
- **Description**: Open-source alternative to E2B for sandboxed code execution with serverless GPU support.
- **Agent Relevance**: Direct
- **Evidence**: Positioned as an E2B alternative for sandboxed AI code execution with additional GPU support for ML workloads.
- **Key Features**: Sandboxed code execution, serverless GPU support, E2B alternative positioning.
- **Status**: Active
- **Pricing**: Usage-based

---

## Nono (Security Shell for AI Agents)
- **Repo/URL**: Referenced in HuggingFace blog
- **Stars**: N/A
- **Open Source**: Yes
- **Description**: Capability-based security shell using kernel-level primitives (Landlock on Linux, Seatbelt on macOS) to sandbox AI agents.
- **Agent Relevance**: Direct
- **Evidence**: Purpose-built for sandboxing AI agents and untrusted processes. Once restrictions are applied, there's no API to escape them. Works at the OS level for maximum security.
- **Key Features**: Landlock (Linux) and Seatbelt (macOS) isolation, capability-based restrictions, no escape API, lightweight.
- **Status**: Active
- **Pricing**: Free (open source)

---

## Summary Statistics

| Metric | Count |
|--------|-------|
| Total entries | 37 |
| Direct agent relevance | 31 |
| Indirect agent relevance | 6 |
| Open source (full or partial) | 24 |
| Proprietary only | 13 |
| Purpose-built for AI agents | 25+ |

### Key Themes

1. **MicroVM dominance**: Firecracker (E2B, Fly.io/Sprites), cloud-hypervisor (Arrakis), and libkrun (microsandbox) are the preferred isolation technologies for agent sandboxes.

2. **Sub-second startup is table stakes**: E2B (80ms), Daytona (90ms), Blaxel (25ms resume), Modal (sub-second), Sprites (1-12s) all compete on startup latency.

3. **Persistence vs ephemeral**: The market is split between ephemeral sandboxes (E2B, Modal) and persistent/stateful ones (Sprites, Daytona, Replit). Sprites explicitly calls ephemeral sandboxes "obsolete."

4. **Self-hosted options growing**: microsandbox, Arrakis, Coder, Northflank (BYOC), and Kubernetes Agent Sandbox serve enterprises needing data sovereignty.

5. **SDK/API convergence**: Rivet Sandbox Agent SDK attempts to unify the fragmented agent API landscape. E2B-compatible APIs (Novita) show de facto standardization.

6. **Agent framework integration**: Nearly all platforms integrate with LangChain, CrewAI, and/or AutoGen. MCP (Model Context Protocol) support is becoming standard.
