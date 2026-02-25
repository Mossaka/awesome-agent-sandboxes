# Container-Based & OCI Runtime Sandboxes for AI Agents

Research compiled: 2026-02-25

This document catalogs container-based sandboxing solutions relevant to AI agent workloads, covering OCI runtimes, container engines, agent-specific container sandboxes, and container-based developer environments.

---

## Table of Contents

1. [Container Engines & High-Level Runtimes](#container-engines--high-level-runtimes)
2. [OCI Low-Level Runtimes](#oci-low-level-runtimes)
3. [Sandboxed / Hardened Container Runtimes](#sandboxed--hardened-container-runtimes)
4. [Agent-Specific Container Sandboxes](#agent-specific-container-sandboxes)
5. [Container-Based Developer / Agent Environments](#container-based-developer--agent-environments)
6. [Container Build Tools (Tangential)](#container-build-tools-tangential)
7. [Multi-Sandbox / Hybrid Runtimes](#multi-sandbox--hybrid-runtimes)
8. [Kubernetes-Native Agent Sandboxes](#kubernetes-native-agent-sandboxes)
9. [Agent Frameworks with Container Sandbox Support](#agent-frameworks-with-container-sandbox-support)

---

## Container Engines & High-Level Runtimes

### Docker (Moby)
- **Repo**: https://github.com/moby/moby
- **Stars**: ~71,500
- **Description**: The open-source container engine that defined modern containerization. Moby is the upstream project for Docker Engine.
- **Agent Relevance**: Direct
- **Evidence**: Docker launched official "Docker Sandboxes" (Nov 2025) specifically for AI coding agents. Provides microVM-based isolation for Claude Code, Gemini, Codex, and Kiro. Docker Sandboxes run agents in lightweight VMs with private Docker daemons, fully isolated from the host.
- **Key Features**: Ubiquitous ecosystem, Docker Compose orchestration, Docker Desktop integration, official AI sandbox support, microVM isolation via Docker Sandboxes
- **Status**: Active (core infrastructure, extremely well-maintained)

### Podman
- **Repo**: https://github.com/containers/podman
- **Stars**: ~30,300
- **Description**: A daemonless, rootless container engine compatible with Docker CLI. Developed by Red Hat.
- **Agent Relevance**: Direct
- **Evidence**: Used as a Docker alternative for running AI agents (Claude Code, Codex) in enterprise environments where Docker Desktop licensing is restrictive. Supported as a backend by llm-sandbox, yolobox, TSK, Code Sandbox MCP, and other agent sandbox tools. Rootless Podman provides stronger default security via user namespaces.
- **Key Features**: Daemonless architecture, rootless by default, Docker CLI compatibility, pod concept (groups of containers), CNCF Sandbox project (Jan 2025)
- **Status**: Active (Red Hat-backed, CNCF project)

### containerd
- **Repo**: https://github.com/containerd/containerd
- **Stars**: ~18,000
- **Description**: An industry-standard container runtime with emphasis on simplicity, robustness, and portability. CNCF graduated project.
- **Agent Relevance**: Indirect (foundational)
- **Evidence**: containerd is the container runtime underneath Docker, Kubernetes (via CRI), and most cloud providers. Its Sandbox API (stable in v2.0) formalizes the integration of sandboxing technologies like gVisor and Kata Containers. GKE Sandbox, Docker Sandboxes, and Northflank all build on containerd. Kuasar implements the containerd Sandbox API.
- **Key Features**: CRI support, Sandbox API (v2.0), shim architecture supporting multiple runtime backends, CNCF graduated status, GPU support via gVisor integration
- **Status**: Active (CNCF graduated, foundational infrastructure)

### LXC / LXD / Incus
- **Repo (Incus)**: https://github.com/lxc/incus
- **Stars**: ~4,900 (Incus)
- **Description**: Incus is a modern system container and VM manager, the community fork of Canonical's LXD. Built on LXC for containers and QEMU for VMs. Provides a unified experience for managing full Linux systems.
- **Agent Relevance**: Potential
- **Evidence**: The "Vibebin" project (Jan 2026) uses Incus/LXC specifically as a platform for self-hosting persistent AI coding agent sandboxes with SSH routing to containers. Incus's system container model provides VM-like isolation with container efficiency, making it well-suited for long-running agent workloads that need full OS environments.
- **Key Features**: System containers (full Linux OS), VM support, clustering (up to 50 servers), REST API, IncusOS (immutable host OS), migration tool from LXD
- **Status**: Active (community-led by original LXD developer Stephane Graber)

---

## OCI Low-Level Runtimes

### runc
- **Repo**: https://github.com/opencontainers/runc
- **Stars**: ~13,000
- **Description**: The reference implementation of the OCI runtime specification. CLI tool for spawning and running containers on Linux.
- **Agent Relevance**: Indirect (foundational)
- **Evidence**: runc is the default low-level runtime used by Docker and containerd. Every standard container runs via runc unless explicitly configured otherwise. It provides namespace/cgroup isolation but shares the host kernel, which is considered insufficient for untrusted AI-generated code execution without additional hardening.
- **Key Features**: OCI reference implementation, namespace/cgroup isolation, seccomp support, widely deployed
- **Status**: Active (core OCI infrastructure)

### crun
- **Repo**: https://github.com/containers/crun
- **Stars**: ~3,600
- **Description**: A fast and lightweight OCI runtime written in C. Default runtime for Podman.
- **Agent Relevance**: Indirect (foundational)
- **Evidence**: Used as the default OCI runtime by Podman. Faster than runc due to C implementation. Relevant as the underlying runtime when Podman is used for agent sandboxing.
- **Key Features**: Written in C (faster than runc), low memory footprint, OCI-compliant, Podman default
- **Status**: Active (Red Hat-maintained)

### youki
- **Repo**: https://github.com/youki-dev/youki
- **Stars**: ~6,400
- **Description**: A container runtime written in Rust, implementing the OCI runtime spec. Memory-safe alternative to runc.
- **Agent Relevance**: Indirect (foundational)
- **Evidence**: Memory-safe Rust implementation reduces the attack surface of the container runtime itself — relevant for security-critical agent sandboxing. Performance between runc and crun.
- **Key Features**: Written in Rust (memory-safe), OCI-compliant, potential for smaller attack surface than Go/C runtimes
- **Status**: Active (community-driven)

---

## Sandboxed / Hardened Container Runtimes

### gVisor (runsc)
- **Repo**: https://github.com/google/gvisor
- **Stars**: ~17,300
- **Description**: Application kernel for containers. Implements a Linux-like interface in user-space (Go), intercepting and servicing syscalls without giving applications direct access to the host kernel.
- **Agent Relevance**: Direct
- **Evidence**: Core sandboxing technology for GKE Sandbox, Modal, Northflank, and kubernetes-sigs/agent-sandbox. Explicitly recommended for AI/ML workloads and LLM-generated code execution. Supports CUDA for GPU workloads. Used in Google Cloud Run, App Engine, and Cloud Functions. The `runsc` binary is an OCI-compatible drop-in replacement for runc.
- **Key Features**: User-space kernel (~200 syscalls implemented), OCI-compatible (runsc), GPU/CUDA support, 20-50% I/O overhead, Kubernetes RuntimeClass integration, written in Go (memory-safe)
- **Status**: Active (Google-maintained, production-proven at scale)

### Sysbox
- **Repo**: https://github.com/nestybox/sysbox
- **Stars**: ~3,400
- **Description**: Next-generation "runc" that enables rootless containers to run system-level workloads (systemd, Docker, Kubernetes) like VMs. Originally by Nestybox, acquired by Docker in 2022.
- **Agent Relevance**: Direct
- **Evidence**: Enables secure Docker-in-Docker without privileged containers — critical for AI agents that need to build/run containers themselves. Docker's own sandbox technology for coding agents builds on Sysbox concepts. Root in container maps to unprivileged user on host, preventing container escape from gaining host privileges.
- **Key Features**: Secure Docker-in-Docker, Kubernetes-in-Docker, user namespace enforcement, VM-like container isolation, no privileged containers needed
- **Status**: Active (Docker-sponsored, community open-source)

---

## Agent-Specific Container Sandboxes

### Docker Sandboxes (Official)
- **Repo**: https://docs.docker.com/ai/sandboxes (closed-source, part of Docker Desktop)
- **Stars**: N/A (part of Docker Desktop)
- **Description**: Docker's official sandbox feature for running AI coding agents in isolated microVM-based environments on your local machine. Released Nov 2025.
- **Agent Relevance**: Direct (purpose-built)
- **Evidence**: Explicitly built for AI coding agents. Supports Claude Code, Gemini, Codex, Kiro. Each sandbox runs in its own microVM with a private Docker daemon. Agents get full access inside but cannot reach the host filesystem, Docker daemon, or other containers.
- **Key Features**: microVM isolation, private Docker daemon per sandbox, workspace mirroring, `docker sandbox` CLI commands, integrated with Docker Desktop
- **Status**: Active (experimental feature, rapidly evolving)

### AIO Sandbox (agent-infra)
- **Repo**: https://github.com/agent-infra/sandbox
- **Stars**: ~2,600
- **Description**: All-in-One Sandbox for AI Agents combining Browser, Shell, File, MCP, and VSCode Server in a single Docker container.
- **Agent Relevance**: Direct (purpose-built)
- **Evidence**: Purpose-built for AI agents with MCP-compatible APIs. Unified file system eliminates multi-sandbox data transfer problems. Built-in VNC browser, VS Code, Jupyter, file manager, terminal. RESTful API under /v1/ namespace with Swagger UI.
- **Key Features**: Unified container (browser + shell + file + MCP + VS Code), persistent tmux sessions, CDP browser connections, Kubernetes/Docker Compose deployment, skill mounting via API
- **Status**: Active (fast iteration, startup time optimized from 16s to 4-8s)

### Alibaba OpenSandbox
- **Repo**: https://github.com/alibaba/OpenSandbox
- **Stars**: ~1,100
- **Description**: General-purpose sandbox platform for AI applications with multi-language SDKs, unified APIs, and Docker/Kubernetes runtimes.
- **Agent Relevance**: Direct (purpose-built)
- **Evidence**: Explicitly supports Coding Agents (Claude Code, Codex, Gemini), GUI Agents, Agent Evaluation, AI Code Execution, and RL Training. Provides examples for Claude Code, Gemini CLI, Google ADK, and Codex CLI running inside sandboxes. Includes Code Interpreter SDK with pre-installed runtimes.
- **Key Features**: Multi-language SDKs (Java/Kotlin, Python, JS/TS), unified sandbox APIs, network policy (ingress gateway + per-sandbox egress), local + Kubernetes deployment, code interpreter built-in
- **Status**: Active (Apache 2.0, Alibaba-backed)

### agent-sandbox/agent-sandbox (E2B-compatible)
- **Repo**: https://github.com/agent-sandbox/agent-sandbox
- **Stars**: ~200 (estimated)
- **Description**: E2B-compatible, enterprise-grade, AI-first, cloud-native runtime for AI Agents on Kubernetes. Self-hosted alternative to E2B.
- **Agent Relevance**: Direct (purpose-built)
- **Evidence**: Fully compatible with E2B protocol and SDKs. Supports untrusted LLM-generated code, browser use, computer use, shell commands. Stateful, long-running, multi-session, multi-tenant. Provides RESTful API and MCP server for sandbox lifecycle management.
- **Key Features**: E2B API compatibility, Kubernetes-native, MCP server, multi-tenant, self-hosted
- **Status**: Active (relatively new)

### Daytona
- **Repo**: https://github.com/daytonaio/daytona
- **Stars**: ~60,100
- **Description**: Secure and elastic infrastructure for running AI-generated code. Pivoted from dev environments to AI agent infrastructure in Feb 2025.
- **Agent Relevance**: Direct (purpose-built)
- **Evidence**: Agent-first architecture with sub-90ms sandbox spin-ups. Native Docker/OCI compatibility. Declarative builder for agents (no local context needed). SDKs in Python and TypeScript. Built-in Git, language servers, file system API, shell command execution.
- **Key Features**: Sub-90ms cold starts, OCI/Docker image support, stateful sandboxes, declarative builder for agents, multi-region support, Python/TS SDKs
- **Status**: Active (heavily funded, fast-growing)

### llm-sandbox
- **Repo**: https://github.com/vndee/llm-sandbox
- **Stars**: ~800 (estimated)
- **Description**: Lightweight and portable LLM sandbox runtime (code interpreter) Python library. Supports Docker, Podman, and Kubernetes backends.
- **Agent Relevance**: Direct (purpose-built)
- **Evidence**: Purpose-built for LLM code execution. Integrates with LangChain, LlamaIndex, OpenAI, Anthropic. Provides MCP server for AI assistants. Container pooling for performance. Multi-language support (Python, JS, Java, C++, Go, R, Ruby).
- **Key Features**: Multi-backend (Docker/Podman/K8s), container pooling, MCP server, auto visualization capture, resource limits, network isolation, MIT license
- **Status**: Active (regular updates, PyPI package)

### Dify Sandbox
- **Repo**: https://github.com/langgenius/dify-sandbox
- **Stars**: ~300 (estimated)
- **Description**: Lightweight, fast, and secure code execution environment for the Dify AI platform. Uses Linux seccomp for syscall restriction.
- **Agent Relevance**: Direct (purpose-built)
- **Evidence**: Part of Dify, a major open-source agentic workflow platform (~70k+ stars). Designed for multi-tenant untrusted code execution. Uses seccomp for syscall filtering and chroot for filesystem isolation. Network isolation via Docker Compose sandbox network.
- **Key Features**: seccomp-based syscall filtering, chroot isolation, Python/Node.js support, multi-tenant design, Docker Compose network isolation
- **Status**: Active (part of Dify ecosystem)

### Dify AgentBox
- **Repo**: https://github.com/langgenius/dify-agentbox
- **Stars**: ~100 (estimated)
- **Description**: Production-ready, all-in-one Docker image for AI agents with multi-language code execution and document processing.
- **Agent Relevance**: Direct (purpose-built)
- **Evidence**: Specifically designed for AI agent code execution. Supports Python, Node.js, Go. Includes document processing capabilities. Part of the Dify ecosystem. Multi-arch builds (amd64, arm64).
- **Key Features**: Multi-language support, document processing, production-ready Docker image, Jinja2-templated Dockerfile
- **Status**: Active (part of Dify ecosystem)

### Yolobox
- **Repo**: https://github.com/finbarr/yolobox
- **Stars**: ~330
- **Description**: Run AI coding agents with full sudo in containers without risking your home directory. Supports Docker and Podman.
- **Agent Relevance**: Direct (purpose-built)
- **Evidence**: Built specifically for running AI coding agents (Claude Code, Codex, Gemini, OpenCode, Copilot) in "YOLO mode" safely. Project directory mounted at real path, home directory NOT mounted. Persistent volumes for tools/configs across sessions.
- **Key Features**: Multi-agent support (Claude, Codex, Gemini, OpenCode, Copilot), Homebrew install, Docker/Podman backends, session continuity, `--no-yolo` flag
- **Status**: Active (regular releases)

### TSK (Task Manager & Sandbox)
- **Repo**: https://github.com/dtormoen/tsk
- **Stars**: ~200 (estimated)
- **Description**: Rust CLI tool for delegating development tasks to AI agents in sandboxed Docker/Podman containers. Returns git branches for review.
- **Agent Relevance**: Direct (purpose-built)
- **Evidence**: Purpose-built task manager for AI coding agents. Parallel execution of agent tasks in isolated containers. Composable Docker images from 4 layers (base + stack + agent + project). Network control via Squid proxy. Git-Town integration for branch workflows.
- **Key Features**: Parallel task execution, composable Docker images, Squid proxy network control, Git-Town integration, daemon-based server, auto-cleanup
- **Status**: Active (Rust CLI)

### Rivet Sandbox Agent SDK
- **Repo**: https://github.com/rivet-dev/sandbox-agent
- **Stars**: ~300 (estimated)
- **Description**: Universal HTTP API for running coding agents in sandboxes. One API for Claude Code, Codex, OpenCode, Amp, and Pi.
- **Agent Relevance**: Direct (purpose-built)
- **Evidence**: Solves the agent API fragmentation problem — each agent has different protocols (JSONL, JSON-RPC, HTTP+SSE). Provides one HTTP API that works with any coding agent. 15MB static Rust binary runs inside any sandbox (E2B, Daytona, Vercel, Docker). Event streaming with persistence.
- **Key Features**: Universal agent API, 15MB Rust binary, event streaming + persistence, Inspector UI, works inside any sandbox runtime
- **Status**: Active (Rivet-backed, open-sourced Jan 2026)

### AgentBox (fletchgqc)
- **Repo**: https://github.com/fletchgqc/agentbox
- **Stars**: ~50 (estimated)
- **Description**: Docker image for running AI agents in YOLO mode. Unified dev environment with Python, Node.js, Java, Shell support.
- **Agent Relevance**: Direct (purpose-built)
- **Evidence**: Purpose-built for running AI coding agents safely. Ephemeral containers with `--rm`. Supports Claude Code, OpenCode, and other agents. Isolated SSH for Git operations. Works with Docker and Podman.
- **Key Features**: Multi-language dev environment, ephemeral containers, isolated SSH, Docker/Podman support, simple lean design
- **Status**: Active (maintained since Sep 2025)

### Code Sandbox MCP (philschmid)
- **Repo**: https://github.com/philschmid/code-sandbox-mcp
- **Stars**: ~100 (estimated)
- **Description**: MCP server providing code interpreter tools (`run_python_code`, `run_javascript_code`) for AI agents using containerized execution via llm-sandbox.
- **Agent Relevance**: Direct (purpose-built)
- **Evidence**: Bridges MCP-compatible AI agents (Claude Desktop, Cursor) with containerized code execution. Network access disabled by default. Resource limits enforced. Built on llm-sandbox.
- **Key Features**: MCP server, Python/JS execution, network disabled by default, resource limits, uses llm-sandbox backend
- **Status**: Active

### Docker Code Sandbox MCP (svngoku)
- **Repo**: https://github.com/svngoku/mcp-docker-code-interpreter
- **Stars**: ~4
- **Description**: MCP server for executing code through Docker sandboxes. Alpine Linux-based containers with security restrictions.
- **Agent Relevance**: Direct (purpose-built)
- **Evidence**: Provides MCP tools (initialize_sandbox, execute_code, stop_sandbox) for AI assistants. Containers have restricted CPU/memory, minimal privileges, network disabled by default. Compatible with Cursor and Claude Desktop.
- **Key Features**: MCP integration, Alpine-based containers, three-tool API, network isolation by default, disposable containers
- **Status**: Active (small project)

### OpenHands
- **Repo**: https://github.com/OpenHands/OpenHands
- **Stars**: ~50,000+ (estimated)
- **Description**: AI-powered coding framework that acts as an autonomous development partner. Uses Docker containers for sandboxed code execution.
- **Agent Relevance**: Direct (purpose-built)
- **Evidence**: First-class Docker runtime support with DockerWorkspace. Sandboxed execution of untrusted code in isolated containers. Supports both local and remote (Docker/Kubernetes) workspace types. Software Agent SDK for building agents with ephemeral Docker workspaces. MIT-licensed.
- **Key Features**: DockerWorkspace for sandboxed execution, Agent Server with container isolation, remote workspace support, VS Code + VNC in Docker, Software Agent SDK, model-agnostic (100+ providers)
- **Status**: Active (major open-source AI agent platform)

---

## Container-Based Developer / Agent Environments

### Distrobox
- **Repo**: https://github.com/89luca89/distrobox
- **Stars**: ~12,100
- **Description**: Use any Linux distribution inside your terminal via Docker/Podman containers. Tightly integrates container with the host.
- **Agent Relevance**: Potential (limited)
- **Evidence**: Provides mutable container environments on immutable OS hosts. Could serve as development environments for AI agents needing specific distro toolchains. NOT designed for sandboxing — explicitly integrates container with host (shares home, network, etc.). Unshare mode provides some isolation but not full security sandbox.
- **Key Features**: Any Linux distro, Podman/Docker backend, declarative config (`distrobox assemble`), GUI app export, unshare mode for partial isolation
- **Status**: Active (GPLv3, pre-installed on SteamOS 3.5+)

### Toolbx (Fedora Toolbox)
- **Repo**: https://github.com/containers/toolbox
- **Stars**: ~2,200 (estimated)
- **Description**: Tool for interactive command line environments on Linux, primarily for immutable OS (Fedora Silverblue, CoreOS). Uses Podman containers.
- **Agent Relevance**: Potential (limited)
- **Evidence**: Similar to Distrobox — provides mutable dev environments on immutable hosts. NOT designed for sandboxing; tightly integrates with host. Could serve as agent development environments but not as security boundaries.
- **Key Features**: Podman-based, seamless host integration, Fedora-native, immutable OS support
- **Status**: Active (Red Hat/Fedora maintained)

### Apptainer (Singularity)
- **Repo**: https://github.com/apptainer/apptainer
- **Stars**: ~1,200 (estimated)
- **Description**: Application containers for Linux, designed for HPC environments. Formerly Singularity. Runs without root, prevents privilege escalation.
- **Agent Relevance**: Potential
- **Evidence**: Strong security model — users inside container are same as outside, no privilege escalation possible. SIF single-file format for reproducible distribution. OCI/Docker image compatibility. Widely used in HPC/scientific computing. Could be relevant for agent workloads on HPC clusters, but no direct AI agent integrations yet.
- **Key Features**: Rootless execution, no privilege escalation, SIF single-file container format, HPC-optimized, Docker/OCI compatibility
- **Status**: Active (Linux Foundation project)

---

## Container Build Tools (Tangential)

### Kaniko
- **Repo**: https://github.com/GoogleContainerTools/kaniko
- **Stars**: ~15,000 (estimated)
- **Description**: Build container images from Dockerfiles inside containers or Kubernetes without requiring a Docker daemon.
- **Agent Relevance**: Indirect
- **Evidence**: Enables agents to build container images in unprivileged environments (e.g., inside Kubernetes pods). No runc dependency, no seccomp/AppArmor needed. Relevant for agent workflows that need to create container images as part of their tasks.
- **Key Features**: Daemonless image building, runs in unprivileged containers, Kubernetes-native, multi-arch support
- **Status**: Active (Google-maintained)

### Buildah
- **Repo**: https://github.com/containers/buildah
- **Stars**: ~7,500 (estimated)
- **Description**: Build OCI images with or without Dockerfiles. Rootless operation, fork-exec model (no daemon).
- **Agent Relevance**: Indirect
- **Evidence**: Enables rootless container image building. Relevant when agents need to create custom container images. Scriptable API beyond Dockerfile syntax.
- **Key Features**: Rootless builds, no daemon, scriptable Go API, OCI-compliant, Dockerfile + non-Dockerfile builds
- **Status**: Active (Red Hat-maintained)

---

## Multi-Sandbox / Hybrid Runtimes

### Kuasar
- **Repo**: https://github.com/kuasar-io/kuasar
- **Stars**: ~2,000 (estimated)
- **Description**: Multi-sandbox container runtime supporting MicroVM (Cloud Hypervisor, StratoVirt, Firecracker, QEMU), app kernel (gVisor-like), Wasm, and runc sandboxes. Written in Rust.
- **Agent Relevance**: Indirect
- **Evidence**: First implementer of containerd v2.0 Sandbox API. Provides unified runtime for multiple sandbox types — useful for platforms that need to offer different isolation levels for different agent workloads. 2x startup speed and 99% overhead reduction vs. traditional shim-per-container model.
- **Key Features**: Multi-sandbox (VMM + app kernel + Wasm + runc), containerd Sandbox API, Rust implementation, single sandboxer process, 2x startup speed
- **Status**: Active (Huawei-originated, CNCF sandbox candidate)

### Microsandbox
- **Repo**: https://github.com/zerocore-ai/microsandbox
- **Stars**: ~4,500
- **Description**: Open-source self-hosted sandboxes for AI agents. Uses libkrun microVMs for hardware-level isolation with sub-200ms boot times.
- **Agent Relevance**: Direct (purpose-built)
- **Evidence**: Purpose-built for AI agents. MCP integration built-in. Uses libkrun (KVM-based library virtualization) — technically microVM but OCI-compatible. Persistent project sandboxes via `./menv` directory. Temporary mode (msx) for ephemeral/stateless tasks. Web automation and browser use supported.
- **Key Features**: libkrun microVMs, OCI-compatible, MCP built-in, sub-200ms boot, persistent + ephemeral modes, self-hosted, Rust implementation
- **Status**: Active (experimental, expect breaking changes)

---

## Kubernetes-Native Agent Sandboxes

### kubernetes-sigs/agent-sandbox
- **Repo**: https://github.com/kubernetes-sigs/agent-sandbox
- **Stars**: ~978
- **Description**: Kubernetes controller providing a declarative API for managing isolated, stateful, singleton workloads for AI agent runtimes. Under SIG Apps.
- **Agent Relevance**: Direct (purpose-built)
- **Evidence**: Formal Kubernetes SIG Apps subproject. Sandbox CRD + controller specifically for AI agent runtimes and LLM-generated code execution. Supports gVisor and Kata Containers as isolation backends. WarmPool CRD for sub-second cold starts. Python SDK for programmatic access. OpenTelemetry observability. Used by Alibaba's OpenSandbox.
- **Key Features**: Sandbox CRD/controller, SandboxTemplate + SandboxClaim, WarmPools (sub-1s startup), gVisor/Kata backends, Python SDK, OpenTelemetry, Apache 2.0
- **Status**: Active (Google-led, Kubernetes SIG Apps)

---

## Agent Frameworks with Container Sandbox Support

### smolagents (HuggingFace)
- **Repo**: https://github.com/huggingface/smolagents
- **Stars**: ~15,000 (estimated)
- **Description**: Barebones library for agents that think in code. First-class support for Code Agents that write actions as Python code.
- **Agent Relevance**: Direct
- **Evidence**: Built-in Docker sandbox support via `executor_type="docker"`. Also supports E2B, Modal, Blaxel, and Pyodide+Deno sandboxes. Docker containers configured with security constraints (512MB memory, CPU quota, PID limit, no-new-privileges, all capabilities dropped, runs as nobody).
- **Key Features**: `executor_type="docker"` for sandboxed execution, multiple sandbox backends, security constraints, context manager cleanup, HuggingFace ecosystem
- **Status**: Active (HuggingFace-maintained)

### Open Interpreter
- **Repo**: https://github.com/OpenInterpreter/open-interpreter
- **Stars**: ~55,000+ (estimated)
- **Description**: Natural language interface for executing code on your computer. Experimental Docker support for isolated execution.
- **Agent Relevance**: Direct
- **Evidence**: Docker support is experimental — all code execution in containers with no access to host system. Working toward making Docker isolation a core feature. Currently the most popular open-source code interpreter.
- **Key Features**: Natural language code execution, experimental Docker isolation, broad language support
- **Status**: Active (Docker support still experimental)

---

## Summary Statistics

| Category | Count |
|----------|-------|
| Container Engines & High-Level Runtimes | 4 |
| OCI Low-Level Runtimes | 3 |
| Sandboxed / Hardened Runtimes | 2 |
| Agent-Specific Container Sandboxes | 14 |
| Developer / Agent Environments | 3 |
| Container Build Tools | 2 |
| Multi-Sandbox / Hybrid Runtimes | 2 |
| Kubernetes-Native Agent Sandboxes | 1 |
| Agent Frameworks with Container Support | 2 |
| **Total** | **33** |

## Key Observations

1. **Containers alone are NOT considered sufficient** for AI agent sandboxing — virtually every source emphasizes that standard containers share the host kernel and are vulnerable to escape. The industry consensus is that additional isolation (gVisor, microVMs, or seccomp hardening) is required.

2. **gVisor (runsc) is the most widely adopted sandboxing layer** for container-based agent workloads, used by GKE Sandbox, Modal, Northflank, and kubernetes-sigs/agent-sandbox.

3. **Docker Sandboxes (Nov 2025)** represent a major shift — Docker itself now provides microVM-based isolation specifically for AI coding agents, validating the agent sandboxing use case.

4. **The agent-specific sandbox ecosystem exploded in 2025-2026**, with purpose-built tools like Daytona, AIO Sandbox, OpenSandbox, llm-sandbox, yolobox, TSK, and Rivet Sandbox Agent all emerging in this period.

5. **MCP (Model Context Protocol) integration** is becoming the standard interface for connecting AI agents to sandbox environments, with llm-sandbox, AIO Sandbox, OpenSandbox, microsandbox, and multiple MCP servers all supporting it.

6. **Kubernetes is becoming the orchestration standard** for agent sandboxes at scale, with kubernetes-sigs/agent-sandbox providing a formal CRD/controller and projects like OpenSandbox, agent-sandbox, and Kuasar providing K8s-native solutions.
