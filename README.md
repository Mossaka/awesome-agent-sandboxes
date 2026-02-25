# Awesome Agent Sandboxes

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated list of sandboxing technologies for AI agents — containers, microVMs, WebAssembly, OS-level isolation, and more.

As AI agents gain the ability to write and execute code, browse the web, and interact with external systems, **sandboxing** has become a critical infrastructure concern. An agent sandbox isolates untrusted agent-generated code from the host system, preventing data exfiltration, resource abuse, and lateral movement — while still providing the agent with a useful execution environment.

This list catalogs the landscape of sandbox technologies relevant to AI agent workloads, from purpose-built agent platforms like E2B and Daytona, to foundational isolation primitives like Firecracker microVMs, gVisor, and Landlock. Projects are organized by their underlying isolation mechanism. Whether you're building an agent framework, deploying agents in production, or researching sandbox security, this list is a starting point.

Key properties that matter for agent sandboxing: **startup latency** (agents need fast iteration), **resource overhead** (many concurrent sandboxes), **security boundary strength** (untrusted code execution), **API surface** (MCP, SDK, REST), **language/runtime support** (Python, JS, shell), and **ease of orchestration** (K8s, Docker, cloud).

## Contents

- [Agent-Native Sandbox Platforms](#agent-native-sandbox-platforms)
  - [Open Source](#open-source)
  - [Cloud & Proprietary](#cloud--proprietary)
- [MicroVM & Hypervisor-Based](#microvm--hypervisor-based)
  - [Virtual Machine Monitors](#virtual-machine-monitors)
  - [Unikernels](#unikernels)
- [Container-Based](#container-based)
  - [Container Engines](#container-engines)
  - [Agent-Specific Container Sandboxes](#agent-specific-container-sandboxes)
- [WebAssembly-Based](#webassembly-based)
  - [Wasm Runtimes](#wasm-runtimes)
  - [Wasm Agent Sandboxes](#wasm-agent-sandboxes)
- [OS-Level & Process Isolation](#os-level--process-isolation)
- [Language Runtime Sandboxes](#language-runtime-sandboxes)
  - [JavaScript / TypeScript](#javascript--typescript)
  - [Python](#python)
  - [Code Execution Engines](#code-execution-engines)
- [AI Coding Agents with Notable Sandbox Architecture](#ai-coding-agents-with-notable-sandbox-architecture)
- [Kubernetes-Native](#kubernetes-native)
- [Confidential Computing & TEEs](#confidential-computing--tees)
- [Novel & Research](#novel--research)
  - [Experimental Projects](#experimental-projects)
  - [Academic Papers](#academic-papers)
- [Archived / Superseded](#archived--superseded)
- [Landscape Overview](#landscape-overview)
- [Contributing](#contributing)

---

## Agent-Native Sandbox Platforms

Projects purpose-built for AI agent sandboxing. These platforms provide managed or self-hosted sandbox environments designed specifically for running AI-generated code safely.

### Open Source

- **[Daytona](https://github.com/daytonaio/daytona)** - Secure and elastic infrastructure for running AI-generated code. `stars: 60.4k` `Go`
  - Pivoted to AI agent infra (Feb 2025). $24M Series A. Sub-90ms sandbox creation. Used by LangChain, Turing, Writer.

- **[E2B](https://github.com/e2b-dev/E2B)** - Open-source cloud runtime for AI agents powered by Firecracker microVMs. `stars: 11k` `Python/TypeScript`
  - $21M Series A. Sub-200ms startup. Used by Manus AI, Groq, HuggingFace, Perplexity. 88% of Fortune 100 signed up.

- **[Microsandbox](https://github.com/zerocore-ai/microsandbox)** - Self-hosted microVM sandboxes for AI agents with MCP integration. `stars: 4.8k` `Rust`
  - Uses libkrun microVMs. Sub-200ms boot. Built-in MCP server. Self-hosted alternative to E2B.

- **[AIO Sandbox](https://github.com/agent-infra/sandbox)** - All-in-one Docker sandbox with Browser, Shell, File, MCP, and VS Code Server for AI agents. `stars: 2.6k` `Python/TypeScript/Go`
  - SDKs in Python/TS/Go. MCP-compatible APIs. Comprehensive agent environment in a single container.

- **[Beam Cloud (beta9)](https://github.com/beam-cloud/beta9)** - Open-source serverless runtime with sandbox for running LLM-generated code. `stars: 1.6k` `Go/Python`
  - Isolated container execution. Serverless GPU support. AGPL-3.0.

- **[Alibaba OpenSandbox](https://github.com/alibaba/OpenSandbox)** - General-purpose sandbox platform for AI applications with multi-language SDKs. `stars: 1.2k` `Java/Python/TypeScript`
  - Alibaba-backed. Examples for Claude Code, Gemini CLI, Codex CLI. Docker and K8s runtimes.

- **[Dify Sandbox](https://github.com/langgenius/dify-sandbox)** - Lightweight code execution sandbox for the Dify AI platform using seccomp. `stars: 1.1k` `Go`
  - Part of Dify ecosystem (~70k+ stars). Seccomp-based syscall filtering + chroot isolation. Multi-tenant.

- **[Cloudflare Sandbox SDK](https://github.com/cloudflare/sandbox-sdk)** - Secure isolated environments on Cloudflare's edge for AI agent code execution. `stars: 910` `TypeScript`
  - Launched June 2025. V8 isolate infrastructure underneath. Beta, rapidly evolving.

- **[Rivet Sandbox Agent SDK](https://github.com/rivet-dev/sandbox-agent)** - Universal HTTP API for running coding agents (Claude Code, Codex, Amp) in any sandbox. `stars: 938` `Rust`
  - Solves agent API fragmentation with a single HTTP API. Static binary. Open-sourced Jan 2026.

- **[Arrakis](https://github.com/abshkbh/arrakis)** - Self-hosted microVM sandbox with snapshot/restore for agent backtracking. `stars: 763` `Go`
  - Uses Cloud Hypervisor. MCP server + Python SDK + REST API. Unique snapshot/restore for MCTS-style agents.

- **[nono](https://github.com/always-further/nono)** - Kernel-enforced sandbox CLI/SDKs for AI agents with capability-based isolation and audit trails. `stars: 586` `Go`
  - Created by Luke Hinds (Sigstore creator). Landlock (Linux) + Seatbelt (macOS). Ships profiles for Claude Code, Codex.

- **[CodeSandbox SDK / Together](https://github.com/codesandbox/codesandbox-sdk)** - Programmatic API for microVM-based dev environments, now part of Together AI. `stars: 101` `TypeScript`
  - MicroVM infrastructure with snapshot/restore. Together AI acquisition validates significance.

### Cloud & Proprietary

- **[Docker Sandboxes](https://docs.docker.com/ai/sandboxes/)** - Docker's official microVM-based sandbox for AI coding agents. `proprietary`
  - Released Dec 2025. Supports Claude Code, Gemini CLI, Codex, Copilot, Kiro. Private Docker daemon per sandbox.

- **[Modal Sandboxes](https://modal.com/products/sandboxes)** - Python-first cloud platform with gVisor sandboxes for AI-generated code. `proprietary`
  - Used by Ramp, Quora/Poe, Codegen. SWE-bench support. Sub-second startup.

- **[Fly.io / Sprites](https://sprites.dev)** - Firecracker microVMs with persistent, stateful sandboxes for AI coding agents. `proprietary`
  - Launched Jan 2026. Claude Code, Gemini, Codex pre-installed. Checkpoint/restore ~300ms.

- **[Blaxel](https://blaxel.ai)** - Perpetual sandbox platform with <25ms resume via Firecracker microVMs. `proprietary`
  - YC Spring 2025. $7.3M seed (First Round + YC). Purpose-built for AI agents.

- **[Runloop](https://runloop.ai)** - Enterprise-grade sandbox infrastructure (Devboxes) for AI coding agents at scale. `proprietary`
  - Founded by former Google File System tech lead. SOC2-compliant. LangChain DeepAgents integration.

- **[Northflank](https://northflank.com/product/sandboxes)** - Enterprise cloud platform with Kata/gVisor microVM sandboxes and BYOC support. `proprietary`
  - 2M+ isolated workloads monthly. Production-grade with multi-cloud deployment.

- **[Novita AI Sandbox](https://novita.ai/sandbox)** - E2B-compatible sandbox runtime with per-second billing and sub-200ms startup. `proprietary`
  - Drop-in E2B API compatibility. Competitive pricing model.

- **[Replit Agent](https://replit.com)** - AI coding agent platform with snapshot engine, versioned databases, and Docker sandboxes. `proprietary`
  - Snapshot engine for time-travel and rollback. Long-standing agent platform.

- **[Amazon Bedrock AgentCore](https://aws.amazon.com/bedrock/agentcore/)** - Fully managed AWS service for AI agents to securely execute code in isolated sandboxes. `proprietary`
  - Supports Strands, LangChain, LangGraph, CrewAI frameworks.

- **[Morph Cloud](https://github.com/morph-labs/morphcloud-examples-public)** - Cloud infrastructure for AI agents with environment branching and burst scalability. `stars: ~200` `Python`
  - YC-backed. Environment branching for parallel agent execution.

## MicroVM & Hypervisor-Based

Lightweight virtual machines and hypervisors providing hardware-level isolation. MicroVMs are the gold standard for strong security boundaries with low overhead.

### Virtual Machine Monitors

- **[Firecracker](https://github.com/firecracker-microvm/firecracker)** - Secure and fast microVMs for serverless computing by AWS. `stars: 32.7k` `Rust`
  - <5 MiB memory overhead, ~125ms boot. Foundation for E2B, Fly.io, Vercel Sandbox. Powers AWS Lambda/Fargate.

- **[gVisor (runsc)](https://github.com/google/gvisor)** - Application kernel implementing Linux syscall interface in user-space Go. `stars: 17.8k` `Go`
  - Core tech for GKE Sandbox, kubernetes-sigs/agent-sandbox, Modal. GPU/CUDA support. Default on GKE Autopilot.

- **[Kata Containers](https://github.com/kata-containers/kata-containers)** - Lightweight VMs with container UX using hardware virtualization. `stars: 7.5k` `Rust/Go`
  - Supports QEMU, Cloud Hypervisor, Firecracker backends. Used by kubernetes-sigs/agent-sandbox. OpenInfra Foundation.

- **[Cloud Hypervisor](https://github.com/cloud-hypervisor/cloud-hypervisor)** - Modern cloud VMM written in Rust with CPU/memory hotplug. `stars: 5.3k` `Rust`
  - Used by Arrakis as its VMM. Supported by Kata Containers.

- **[Microsoft LiteBox](https://github.com/microsoft/litebox)** - Security-focused library OS in Rust reducing host attack surface. `stars: 2.4k` `Rust`
  - Released Feb 2026. Supports AMD SEV-SNP confidential computing. Experimental.

### Unikernels

Specialized single-purpose VMs with minimal attack surface. Extreme specialization makes them conceptually ideal for agent isolation.

- **[OSv](https://github.com/cloudius-systems/osv)** - Unikernel running unmodified Linux binaries with ~3-5ms boot. `stars: 4.2k` `C++`
  - Runs unmodified Python/Node.js/Java — the most practical unikernel for agent workloads.

- **[Unikraft](https://github.com/unikraft/unikraft)** - Cloud-native unikernel development kit. Linux Foundation project. `stars: 3.5k` `C`
  - Modular library OS. Build ultra-lightweight purpose-specific VMs.

- **[Nanos (NanoVMs)](https://github.com/nanovms/nanos)** - Single-process unikernel with pledge/unveil sandboxing and GPU support. `stars: 3.1k` `C`
  - ~5ms boot on Firecracker. Single-application design inherently suited for agent isolation.

## Container-Based

Container technologies providing namespace and cgroup-based isolation. Widely used as the default isolation layer for agent workloads, though containers alone (shared kernel) are generally considered insufficient for untrusted AI code without additional hardening.

### Container Engines

- **[Docker (Moby)](https://github.com/moby/moby)** - The container engine that defined modern containerization. `stars: 71.5k` `Go`
  - Docker launched official "Docker Sandboxes" (Dec 2025) for AI coding agents. Ubiquitous ecosystem.

- **[Podman](https://github.com/containers/podman)** - Daemonless, rootless container engine. Docker CLI-compatible. `stars: 30.8k` `Go`
  - Red Hat-backed. Rootless security via user namespaces. Backend for llm-sandbox, Yolobox, TSK.

- **[LXC / Incus](https://github.com/lxc/incus)** - System container and VM manager. Community fork of LXD. `stars: 4.9k` `Go`
  - Full OS environment isolation. Used by Vibebin for persistent AI coding agent sandboxes with SSH routing.

- **[Sysbox](https://github.com/nestybox/sysbox)** - Container runtime enabling rootless Docker-in-Docker without privileges. `stars: 3.5k` `Go`
  - Docker-sponsored. Critical for agents that need to build/run containers inside sandboxes.

### Agent-Specific Container Sandboxes

- **[llm-sandbox](https://github.com/vndee/llm-sandbox)** - Python library for LLM code execution with Docker/Podman/K8s backends and MCP server. `stars: 834` `Python`
  - Integrates with LangChain, LlamaIndex, OpenAI, Anthropic. Multi-language execution.

- **[Yolobox](https://github.com/finbarr/yolobox)** - Run AI coding agents with full sudo in Docker/Podman containers safely. `stars: 451` `Shell`
  - Explicitly for Claude Code, Codex, Gemini, OpenCode, Copilot. Homebrew installable.

- **[SWE-ReX](https://github.com/SWE-agent/SWE-ReX)** - Runtime interface for sandboxed shell environments for AI agents. `stars: 442` `Python`
  - Powers SWE-agent. Sandboxed code execution specifically for AI agent workflows.

- **[TSK](https://github.com/dtormoen/tsk)** - Rust CLI for delegating dev tasks to AI agents in sandboxed Docker/Podman containers. `stars: ~200` `Rust`
  - Unique git-branch-based workflow. Parallel task execution. Network control via Squid proxy.

- **[Dify AgentBox](https://github.com/langgenius/dify-agentbox)** - Production Docker image for AI agents with multi-language code execution. `stars: ~100` `Python`
  - Part of Dify ecosystem. Python, Node.js, Go execution + document processing.

- **[agent-sandbox (E2B-compatible)](https://github.com/agent-sandbox/agent-sandbox)** - Self-hosted, Kubernetes-native, E2B-compatible runtime for AI agents. `stars: 64` `Go`
  - Drop-in E2B API compatibility. MCP server. Multi-tenant.

## WebAssembly-Based

WebAssembly provides capability-based sandboxing with near-native performance. The deny-by-default security model makes it architecturally suited for running untrusted agent code.

### Wasm Runtimes

- **[Wasmer](https://github.com/wasmerio/wasmer)** - Fast, secure WebAssembly runtime. No file, network, or environment access unless explicitly enabled. `stars: 20.4k` `Rust`
  - Sandbox-by-default. Multiple compilation backends (Cranelift, LLVM, Singlepass).

- **[Wasmtime](https://github.com/bytecodealliance/wasmtime)** - Fast, secure, standards-compliant Wasm runtime by Bytecode Alliance. `stars: 17.6k` `Rust`
  - Foundation for Wassette, amla-sandbox, and many agent sandboxes. Cranelift JIT/AOT.

- **[WasmEdge](https://github.com/WasmEdge/WasmEdge)** - Lightweight Wasm runtime. CNCF Sandbox project with LlamaEdge for on-device LLMs. `stars: 10.5k` `C++`
  - WASI-NN support for AI inference. Cloud-native with Kubernetes integration.

- **[workerd](https://github.com/cloudflare/workerd)** - V8 isolate-based JavaScript/Wasm runtime powering Cloudflare Workers. `stars: 7.5k` `C++`
  - Powers Cloudflare Sandbox SDK infrastructure. Sub-millisecond cold starts. WASI support.

- **[Spin](https://github.com/spinframework/spin)** - Framework for building fast, secure cloud microservices with WebAssembly. `stars: 6.3k` `Rust`
  - CNCF Sandbox project (Akamai). Sub-millisecond cold starts. Serverless AI API support.

- **[Extism](https://github.com/extism/extism)** - Universal WebAssembly plugin framework for executing untrusted code safely. `stars: 5.4k` `Rust`
  - 16+ host language SDKs. Designed for safe arbitrary code execution.

- **[wazero](https://github.com/tetratelabs/wazero)** - Zero-dependency WebAssembly runtime for Go. Pure Go, no CGO. `stars: 5.1k` `Go`
  - Foundation for Go-based Wasm sandboxing. No external dependencies.

- **[wasmCloud](https://github.com/wasmCloud/wasmCloud)** - CNCF Incubating project for polyglot Wasm apps with capability-based security. `stars: 2.2k` `Rust`
  - Cosmonic Sandbox MCP plugin built on wasmCloud for agent sandboxing.

- **[Golem Cloud](https://github.com/golemcloud/golem)** - Agent-native platform with durable computing on WebAssembly. `stars: 964` `Rust`
  - Self-describes as "agent-native platform." Durable execution — state survives failures. MCP support.

### Wasm Agent Sandboxes

- **[Wassette](https://github.com/microsoft/wassette)** - Security-oriented runtime executing WebAssembly Components via MCP. Built on Wasmtime. `stars: 846` `Rust`
  - Microsoft. Browser-grade isolation. Works with GitHub Copilot, Claude Code, Cursor, Gemini CLI.

- **[amla-sandbox](https://github.com/amlalabs/amla-sandbox)** - WASM sandbox with capability enforcement for AI agents. QuickJS inside WASM. `stars: 309` `Rust`
  - Unforgeable capability tokens. Targets LangChain, AutoGen, SWE-Agent frameworks.

- **[Sandbox MCP (Cosmonic)](https://sandboxmcp.ai)** - Plugin for wasmCloud generating MCP servers as secure Wasm components from OpenAPI specs. `proprietary`
  - Converts OpenAPI specs to sandboxed MCP tools. Tightly coupled to wasmCloud ecosystem.

## OS-Level & Process Isolation

Operating system primitives and tools for sandboxing processes without full virtualization. Lower overhead than VMs but weaker isolation boundaries.

- **[Firejail](https://github.com/netblue30/firejail)** - SUID sandbox using namespaces, seccomp-bpf, capabilities. 1000+ app profiles. `stars: 7.1k` `C`
  - Mature general-purpose sandbox. Landlock support (kernel 5.13+).

- **[Bubblewrap (bwrap)](https://github.com/containers/bubblewrap)** - Low-level unprivileged sandboxing tool. Used by Flatpak. `stars: 5.9k` `C`
  - **Used by Claude Code** on Linux. Significantly faster than Docker for lightweight sandboxing.

- **[nsjail](https://github.com/google/nsjail)** - Lightweight process isolation using namespaces, cgroups, rlimits, and seccomp-bpf. `stars: 3.7k` `C++`
  - Google. Used by Windmill for sandboxing code execution. Kafel DSL for seccomp policies.

- **[landrun](https://github.com/Zouuup/landrun)** - Run any Linux process in a Landlock sandbox. Single binary, no root. `stars: 2.1k` `Go`
  - Practical CLI wrapper for Landlock. Referenced in AI agent sandboxing discussions.

- **[Sandboxed API / Sandbox2](https://github.com/google/sandboxed-api)** - Auto-generates sandboxes for C/C++ libraries. Sandbox2 is the underlying engine. `stars: 1.7k` `C++`
  - Google Sandbox Team. Used internally for production workloads.

- **[macOS Sandbox (Seatbelt)](https://github.com/s7ephen/OSX-Sandbox--Seatbelt--Profiles)** - macOS kernel-level sandboxing via sandbox-exec with Seatbelt profile language. `stars: ~200` `SBPL`
  - Used by OpenAI Codex, Claude Code, and nono on macOS. Deprecated but functional and actively used.

- **[Landlock LSM](https://github.com/landlock-lsm)** - Linux kernel module for unprivileged process filesystem and network access restriction. `kernel`
  - Used by **OpenAI Codex** and **Cursor** for agent sandboxing. Available since kernel 5.13. ABI v5 in Linux 6.7+. ([rust-landlock](https://github.com/landlock-lsm/rust-landlock) `stars: 201`, [go-landlock](https://github.com/landlock-lsm/go-landlock) `stars: 276`)

- **[sandboxec](https://github.com/dwisiswant0/sandboxec)** - Lightweight single-binary Landlock sandbox with MCP mode for AI agent tool integration. `stars: 45` `Go`
  - Very early stage (Feb 2026). Novel MCP mode for integrating with agent tool chains.

## Language Runtime Sandboxes

Language runtimes and interpreters with built-in sandboxing capabilities, enabling safe execution of agent-generated code in specific languages.

### JavaScript / TypeScript

- **[Deno](https://github.com/denoland/deno)** - Modern JS/TS runtime with built-in permissions-based security. `stars: 106k` `Rust`
  - Granular permission flags (--allow-read, --allow-net, etc.). Deno Sandbox cloud product uses Firecracker for AI agents (used by LangChain, Pydantic AI).

- **[Bun](https://github.com/oven-sh/bun)** - Fast all-in-one JavaScript runtime. Acquired by Anthropic. `stars: 87.6k` `Zig`
  - No sandbox feature yet. Permissions model proposed (Issue #25929). Anthropic acquisition signals agent sandbox trajectory.

- **[isolated-vm](https://github.com/laverdet/isolated-vm)** - Access V8's Isolate interface from Node.js — true isolate separation with separate heaps. `stars: ~2.7k` `C++`
  - Recommended vm2 replacement. Used by Temporal SDK. Real V8 isolate separation.

- **[quickjs-emscripten](https://github.com/justjake/quickjs-emscripten)** - Safely execute untrusted JavaScript via QuickJS compiled to WebAssembly. `stars: 1.6k` `TypeScript`
  - 112k+ weekly npm downloads. Foundation for higher-level JS sandboxes.

- **[@sebastianwessel/quickjs](https://github.com/sebastianwessel/quickjs)** - TypeScript package for executing JS/TS in a WebAssembly QuickJS sandbox with virtual filesystem. `stars: 867` `TypeScript`
  - Has QuickJS Sandbox MCP server. v3.0.1.

### Python

- **[Pyodide](https://github.com/pyodide/pyodide)** - CPython compiled to WebAssembly for browser and Node.js. `stars: 14.2k` `Python/C`
  - Foundation for smolagents, mcp-run-python. NVIDIA recommends for agentic AI sandboxing.

- **[Pydantic Monty](https://github.com/pydantic/monty)** - Minimal, secure Python interpreter in Rust for AI. Microsecond startup. `stars: 5.7k` `Rust`
  - "Safely run Python code written by an LLM." No filesystem/network access by default. Successor to mcp-run-python.

### Code Execution Engines

- **[Judge0](https://github.com/judge0/judge0)** - Robust online code execution system for humans and AI. 90+ languages. `stars: 4k` `Ruby`
  - AI agents listed as use case. MCP server available. Docker + Isolate sandboxing.

- **[Piston](https://github.com/engineer-man/piston)** - High-performance code execution engine using namespaces, chroot, cgroups. 70+ languages. `stars: 2.6k` `JavaScript`
  - MCP server available (piston-mcp). API restricted to non-commercial use as of Feb 2026.

## AI Coding Agents with Notable Sandbox Architecture

These are not sandboxes themselves, but their sandbox architecture decisions are influential and worth studying.

- **[OpenHands](https://github.com/All-Hands-AI/OpenHands)** - AI-powered autonomous development platform with Docker sandbox isolation. `stars: 68.2k` `Python`
  - Formerly OpenDevin. $18.8M Series A. Docker sandbox per session. Architecture widely referenced.

- **[OpenAI Codex](https://github.com/openai/codex)** - Cloud-based AI coding agent with Landlock (Linux) and Seatbelt (macOS) sandboxing. `stars: 61.9k` `TypeScript`
  - Pioneer of OS-level agent sandboxing. Configurable sandbox modes. MCP support.

- **[smolagents](https://github.com/huggingface/smolagents)** - HuggingFace library for Code Agents with 5 sandbox backends. `stars: 25.6k` `Python`
  - Docker, E2B, Modal, Blaxel, Pyodide+Deno sandboxes. Notable for supporting the broadest range of sandbox backends.

## Kubernetes-Native

Kubernetes-specific solutions for managing AI agent sandbox workloads at scale.

- **[kubernetes-sigs/agent-sandbox](https://github.com/kubernetes-sigs/agent-sandbox)** - Kubernetes CRD/controller for managing isolated AI agent runtimes. `stars: 1.1k` `Go`
  - CNCF SIG Apps subproject. Launched at KubeCon NA 2025 (Google). Uses gVisor + Kata Containers. WarmPools for sub-second startup.

## Confidential Computing & TEEs

Trusted Execution Environments for running agent workloads with hardware-attested privacy guarantees.

- **[Gramine](https://github.com/gramineproject/gramine)** - Library OS for running unmodified Linux apps inside Intel SGX enclaves. `stars: ~1.5k` `C`
  - RA-TLS attestation. Used for confidential AI inference.

- **[Occlum](https://github.com/occlum/occlum)** - Memory-safe library OS for Intel SGX written in Rust. `stars: ~1.5k` `Rust`
  - 1000x faster enclave startup vs alternatives. Relevant for confidential agent execution.

- **[Enarx](https://github.com/enarx/enarx)** - Confidential Computing with WebAssembly in TEEs (Intel SGX, AMD SEV-SNP). `stars: 1.4k` `Rust`
  - Bridges Wasm and TEE ecosystems. Low activity but not archived.

## Novel & Research

### Experimental Projects

- **[awesome-sandbox](https://github.com/restyler/awesome-sandbox)** - Curated list and analysis of modern code sandboxing solutions for AI. `stars: 352`
  - Community resource with comprehensive comparison of sandbox solutions. AI agent focus.

- **[Azazel](https://github.com/beelzebub-labs/azazel)** - eBPF-powered silent observer for containerized runtimes — monitors every syscall, file access, network connection. `stars: 61` `Go`
  - Designed for "Agentic AI monitoring." Zero-overhead eBPF tracing. Observability complement to sandboxing.

### Academic Papers

1. **Fault-Tolerant Sandboxing for AI Coding Agents** (Dec 2025) — [arxiv.org/abs/2512.12806](https://arxiv.org/abs/2512.12806)
2. **AgentBound: Policy Enforcement for AI Agent Execution** (Oct 2025) — [arxiv.org/pdf/2510.21236](https://arxiv.org/pdf/2510.21236)
3. **LLM-in-Sandbox Elicits General Agentic Intelligence** (Jan 2026) — [arxiv.org/pdf/2601.16206](https://arxiv.org/pdf/2601.16206)
4. **Systems Security Foundations for Agentic Computing** (2025) — [eprint.iacr.org/2025/2173.pdf](https://eprint.iacr.org/2025/2173.pdf)
5. **RedCodeAgent: Automated Red-Teaming for Code Agents** — [openreview.net](https://openreview.net)

## Archived / Superseded

Notable projects that have been archived but provide historical context.

- ⚠️ **[mcp-run-python (Pydantic)](https://github.com/pydantic/mcp-run-python)** - Archived Jan 30, 2026. `stars: 189`
  - "No safe way to run Python within Pyodide safely with reasonable latency." Superseded by **Pydantic Monty**.

- ⚠️ **[LangChain Sandbox](https://github.com/langchain-ai/langchain-sandbox)** - Archived Jan 14, 2026. `stars: 234`
  - "No longer maintained. Recommend accessing code execution through sandbox APIs." Superseded by **LangChain DeepAgents**.

## Landscape Overview

The agent sandbox ecosystem has matured rapidly since mid-2025. Key trends:

**Firecracker dominance.** AWS Firecracker microVMs are the de facto isolation technology for cloud agent sandboxes. E2B, Fly.io/Sprites, Blaxel, Deno Sandbox, and Docker Sandboxes all build on Firecracker or Firecracker-derived technology. Its <5 MiB overhead and ~125ms boot make it uniquely suited for ephemeral agent workloads.

**MCP convergence.** The Model Context Protocol is becoming the standard interface between agents and sandboxes. Wassette, microsandbox, sandboxec, Sandbox MCP, llm-sandbox, Judge0, Piston, and several others now expose MCP servers, allowing agents to discover and use sandbox capabilities through a unified protocol.

**Landlock + Seatbelt for lightweight isolation.** For CLI-based agents that don't need full containers, OS-level sandboxing is gaining traction. OpenAI Codex and Cursor use Landlock on Linux, while Claude Code uses Bubblewrap. On macOS, Seatbelt (sandbox-exec) is the common primitive used by Codex, Claude Code, and nono.

**Significant VC investment.** The agent sandbox space has attracted over $70M in venture capital: E2B ($21M Series A), Daytona ($24M Series A), OpenHands ($18.8M Series A), Blaxel ($7.3M seed). This signals strong market conviction that sandboxing is critical infrastructure for the agent economy.

**gVisor as the Kubernetes standard.** Google's gVisor is the default container sandbox on GKE Autopilot and the core technology behind kubernetes-sigs/agent-sandbox, Modal, and Northflank. Its user-space kernel implementation provides strong isolation without full VM overhead.

**WebAssembly for fine-grained isolation.** Wasm's capability-based security model is gaining traction for agent tool sandboxing. Wassette (Microsoft), amla-sandbox, and Pydantic Monty (Pyodide successor) use Wasm to sandbox individual tool invocations rather than entire environments.

**Snapshot/restore as a pattern.** Arrakis, Replit, Morph Cloud, and Fly.io/Sprites offer environment snapshotting, enabling agents to checkpoint state and roll back — critical for MCTS-style agent architectures that explore multiple solution paths.

**Self-hosted vs. cloud.** The market is splitting between cloud platforms (E2B, Modal, Blaxel) and self-hosted solutions (microsandbox, Arrakis, nono). Self-hosted options are emerging for enterprises with data sovereignty requirements.

## Contributing

Contributions welcome! Please read the following guidelines:

- **When adding a new project**, include: name, GitHub/project link, technology category, star count, language, a brief description, and one line about agent relevance.
- **Categorize by isolation mechanism**: container, microVM, Wasm, OS-level, language runtime, etc.
- **Focus on agent-relevant properties**: startup latency, resource overhead, security boundary strength, API surface (MCP, SDK, REST), language/runtime support, and orchestration.
- **Verify before adding**: confirm the project exists, is not archived (unless noting it as such), and has meaningful adoption or a novel approach.
- **Be selective**: Don't list foundational building blocks that are already subsumed by higher-level tools on this list (e.g., don't add runc separately if it's already inside Docker). Each entry should stand on its own as something you'd directly use or evaluate for agent sandboxing.
- Mark archived projects with ⚠️.

---

*Last updated: February 2026*
