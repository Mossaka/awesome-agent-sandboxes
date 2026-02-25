# Verified Infrastructure Sandboxes (MicroVM + Container + OS-Level)

Verification conducted: 2026-02-25
Sources: microvm-research.md, container-research.md, os-security-research.md

## Duplicates Found

The following projects appeared as full entries in multiple research files:

1. **gVisor** — listed in microvm-research.md (#3) AND container-research.md (Sandboxed/Hardened section). Canonical category: Container-Based (it's an application kernel providing container isolation, not a hypervisor).
2. **Kuasar** — listed in microvm-research.md (#6) AND container-research.md (Multi-Sandbox section). Canonical category: Container-Based (multi-sandbox runtime).
3. **Microsandbox** — listed in microvm-research.md (#8) AND container-research.md (Multi-Sandbox section). Canonical category: MicroVM (uses libkrun microVMs under the hood).
4. **kubernetes-sigs/agent-sandbox** — listed in microvm-research.md (#28) AND container-research.md (K8s-Native section). Canonical category: Kubernetes-Native (its own category).

## Corrections Found During Verification

1. **Landlock**: Research claimed "used by Claude Code" — **INCORRECT**. Claude Code uses Bubblewrap (bwrap) on Linux, not Landlock. Landlock is used by OpenAI Codex and Cursor.
2. **Docker Sandboxes**: Research claimed "Nov 2025" launch — **CORRECTED** to December 2025.
3. **OpenHands**: Research claimed "~50,000+" stars — **CORRECTED** to ~68,200 stars. Repo is at All-Hands-AI/OpenHands.
4. **E2B**: Stars corrected from ~8,900 to ~11,000.
5. **smolagents**: Stars corrected from ~15,000 to ~25,600.
6. **LiteBox**: Stars corrected from ~330 to ~2,400.
7. **Windows Sandbox**: Stars corrected from ~2,000 to ~512.
8. **sandboxec**: Stars corrected from ~100+ to ~45.
9. **CodeDuet**: Stars corrected from <100 to ~3. Enterprise claims unverifiable.

## Excluded Entries (with reasons)

| Project | Reason for Exclusion |
|---------|---------------------|
| Docker Code Sandbox MCP (svngoku) | ~4 stars, trivial MCP wrapper, no novel approach |
| seccomp-pledge (SubconsciousCompute) | ~9 stars, experimental, no novel approach |
| sandboxlib (CodethinkLabs) | ~8 stars, stale since Dec 2022 |
| CodeDuet microvm-ai-agent-sandbox | ~3 stars, 15 commits, enterprise claims unverifiable |
| Distrobox | Explicitly "NOT designed for sandboxing" — shares host home/network |
| Toolbx (Fedora Toolbox) | Not designed for sandboxing — tight host integration |
| fakechroot | LD_PRELOAD interception, no security guarantees |
| udocker | "Does NOT offer robust isolation" per its own docs |
| Nabla Containers | Stale since 2019-2020, limited to custom images |
| Mbox (MIT PDOS) | Stale academic project, ptrace-based |
| Unikernel Linux (UKL) | ~184 stars, research-only, no agent relevance |
| seccompiler (rust-vmm) | ~102 stars, internal library crate, too low-level |
| pledge/unveil Linux port (jart) | ~128 stars, thin port, no agent usage |
| Landlock Make | Build tool, not a sandbox tool |
| HP Sure Click / Bromium | Proprietary, no public repo |
| Chromium Sandbox | Not standalone — part of Chromium monorepo |
| SELinux Sandbox | Kernel MAC policy, not a standalone tool |
| RootlessKit | Too foundational (plumbing for rootless Docker) |
| slirp4netns | Too foundational (networking plumbing) |
| PRoot | ptrace-based, not security-focused, performance penalty |
| Capsicum | FreeBSD-only kernel feature, not applicable to most agent workloads |

---

## Verified Entries

---

### Category: MicroVM / Hypervisor

#### Firecracker
- **Repo**: https://github.com/firecracker-microvm/firecracker
- **Stars**: ~32,700 (verified)
- **Description**: Secure and fast microVMs for serverless computing by AWS. Minimal VMM using KVM with <5 MiB memory overhead and ~125ms boot.
- **Agent Relevance**: Direct
- **Verification**: Foundation for E2B, Vercel Sandbox, and multiple agent platforms. Powers AWS Lambda/Fargate. Gold standard for AI agent code execution isolation.
- **Status**: Active (v1.14.1, Jan 2026. Apache-2.0, Rust)
- **Include in final list**: Yes — cornerstone technology

#### Cloud Hypervisor
- **Repo**: https://github.com/cloud-hypervisor/cloud-hypervisor
- **Stars**: ~5,300 (verified)
- **Description**: Modern VMM for cloud workloads written in Rust. Supports CPU/memory hotplug, Windows/Linux guests.
- **Agent Relevance**: Direct
- **Verification**: Used by Arrakis (AI agent sandbox) as its VMM. Supported by Kata Containers and Kuasar. No AI mentions in repo README itself.
- **Status**: Active (9,363 commits, Apache-2.0/BSD-3-Clause, Rust)
- **Include in final list**: Yes — key VMM for agent sandboxes

#### E2B
- **Repo**: https://github.com/e2b-dev/E2B
- **Stars**: ~11,000 (verified, research said ~8,900)
- **Description**: Open-source secure cloud runtime for AI agents powered by Firecracker microVMs with sub-200ms startup.
- **Agent Relevance**: Direct
- **Verification**: VERIFIED — $21M Series A, used by Manus AI, Groq, HuggingFace, Perplexity. 88% of Fortune 100 signed up. Explicitly AI agent-focused.
- **Status**: Active (v2.14.0 Python SDK, Feb 2026. Apache-2.0)
- **Include in final list**: Yes — leading agent sandbox platform

#### Microsandbox
- **Repo**: https://github.com/zerocore-ai/microsandbox
- **Stars**: ~4,800 (verified, research said ~4,500)
- **Description**: Open-source self-hosted sandboxes for AI agents using libkrun microVMs with sub-200ms boot and MCP integration.
- **Agent Relevance**: Direct
- **Verification**: VERIFIED — explicitly designed for AI agents, built-in MCP server documented at docs.microsandbox.dev, listed on Glama MCP platform.
- **Status**: Active (experimental, Apache-2.0, Rust)
- **Include in final list**: Yes — self-hosted alternative to E2B

#### Arrakis
- **Repo**: https://github.com/abshkbh/arrakis
- **Stars**: ~763 (verified, research said ~500+)
- **Description**: Self-hosted sandboxing for AI agent code execution using Cloud Hypervisor. Features snapshot/restore for agent backtracking.
- **Agent Relevance**: Direct
- **Verification**: VERIFIED — featured on Hacker News (Show HN), ai.engineer talk. Created by Abhishek Bhardwaj (ex-Replit/Google/Microsoft). MCP server + Python SDK + REST API.
- **Status**: Active (AGPL-3.0, Go)
- **Include in final list**: Yes — unique snapshot/restore for MCTS agents

#### Kata Containers
- **Repo**: https://github.com/kata-containers/kata-containers
- **Stars**: ~7,500 (verified)
- **Description**: Lightweight VMs that feel like containers with hardware virtualization isolation. Supports QEMU, Cloud Hypervisor, Firecracker.
- **Agent Relevance**: Direct
- **Verification**: Integrated with kubernetes-sigs/agent-sandbox at KubeCon NA 2025. Used by Northflank (2M+ microVMs/month). Supported on Azure AKS.
- **Status**: Active (v3.27.0, Feb 2026. Apache-2.0, Rust/Go, OpenInfra Foundation)
- **Include in final list**: Yes — production-grade VM container isolation

#### QEMU/KVM
- **Repo**: https://github.com/qemu/qemu (mirror)
- **Stars**: ~10,500 (verified)
- **Description**: General-purpose machine emulator and virtualizer. Most feature-complete VMM with extensive device models.
- **Agent Relevance**: Indirect
- **Verification**: Default hypervisor for Kata Containers. Baseline for Firecracker comparisons. Large attack surface limits production agent use.
- **Status**: Active (GPL-2.0, C)
- **Include in final list**: Yes — foundational reference, default Kata backend

#### Kuasar
- **Repo**: https://github.com/kuasar-io/kuasar
- **Stars**: ~1,400 (verified)
- **Description**: Multi-sandbox container runtime by Huawei supporting microVM, app kernel, and Wasm sandboxes. First containerd v2.0 Sandbox API implementer.
- **Agent Relevance**: Indirect
- **Verification**: CNCF sandbox project. Supports Cloud Hypervisor, StratoVirt, Firecracker, QEMU backends. 2x faster startup vs Kata.
- **Status**: Active (v1.0.1, Mar 2025. Apache-2.0, Rust)
- **Include in final list**: Yes — multi-sandbox runtime with agent-relevant architecture

#### libkrun
- **Repo**: https://github.com/containers/libkrun
- **Stars**: ~1,700 (verified, research said ~1,500)
- **Description**: Library-based VMM providing virtualization via KVM (Linux) and HVF (macOS). Powers microsandbox.
- **Agent Relevance**: Indirect
- **Verification**: Powers microsandbox (AI agent sandbox). Used by Podman. Supports AMD SEV / Intel TDX confidential computing.
- **Status**: Active (Apache-2.0, Rust)
- **Include in final list**: Yes — enables microsandbox, portable VM isolation

#### Microsoft LiteBox
- **Repo**: https://github.com/microsoft/litebox
- **Stars**: ~2,400 (verified, research said ~330 — major correction)
- **Description**: Security-focused library OS by Microsoft in Rust. Reduces host attack surface by embedding OS services into applications.
- **Agent Relevance**: Potential
- **Verification**: Released Feb 2026. No direct AI agent usage yet. Supports AMD SEV-SNP. Experimental. Described as "sandbox" in README.
- **Status**: Experimental (MIT, Rust, Microsoft)
- **Include in final list**: Yes — novel approach from Microsoft, active development

#### crosvm
- **Repo**: https://github.com/google/crosvm
- **Stars**: ~1,100 (verified)
- **Description**: Secure lightweight VMM for Chrome OS. Process-per-device sandboxing with Minijail. Rust-based.
- **Agent Relevance**: Potential
- **Verification**: No direct AI agent usage. Used in Chrome OS, Android (ARCVM). Contributes to rust-vmm ecosystem.
- **Status**: Active (BSD-3-Clause, Rust, Google)
- **Include in final list**: Yes — security-first design, Google-backed

#### rust-vmm
- **Repo**: https://github.com/rust-vmm/community
- **Stars**: ~350 (community repo, varies per crate)
- **Description**: Shared Rust virtualization crates used by Firecracker, Cloud Hypervisor, crosvm, libkrun.
- **Agent Relevance**: Indirect
- **Verification**: Foundation layer for most Rust VMMs used in agent sandboxing. Contributors: Amazon, Google, Intel, Red Hat, Alibaba.
- **Status**: Active (Apache-2.0/BSD-3-Clause, Rust)
- **Include in final list**: Yes — foundational ecosystem

#### StratoVirt
- **Repo**: https://github.com/openeuler-mirror/stratovirt
- **Stars**: ~400 (verified)
- **Description**: Enterprise lightweight virtualization by Huawei/openEuler. Supports microVM and standard VM modes.
- **Agent Relevance**: Indirect
- **Verification**: Supported as VMM backend in Kuasar. No direct AI agent usage found.
- **Status**: Active (Mulan PSL v2, Rust)
- **Include in final list**: Yes — but lower priority, mainly as Kuasar backend

#### Dragonball Sandbox
- **Repo**: https://github.com/openanolis/dragonball-sandbox
- **Stars**: ~89 (verified)
- **Description**: Alibaba VMM crates for Kata Containers 3.0. Powers Alibaba Cloud function computing at massive scale.
- **Agent Relevance**: Indirect
- **Verification**: Repository ARCHIVED (July 2023). Technology lives on in Kata Containers. Alibaba Cloud production at 5B calls/day.
- **Status**: Archived
- **Include in final list**: No — archived, technology subsumed into Kata Containers

#### microvm.nix
- **Repo**: https://github.com/microvm-nix/microvm.nix
- **Stars**: ~1,900 (verified)
- **Description**: Nix Flake for building NixOS MicroVMs on 7 hypervisor backends.
- **Agent Relevance**: Indirect
- **Verification**: Used by OpenClaw for AI agent sandboxing. Blog posts demonstrate microvm.nix for AI coding agent sandboxing.
- **Status**: Active (MIT, Nix)
- **Include in final list**: Yes — practical multi-hypervisor orchestration

#### Alioth
- **Repo**: https://github.com/google/alioth
- **Stars**: ~300 (verified)
- **Description**: Experimental Rust Type-2 hypervisor by Google. AMD SEV/SNP support, cross-platform (KVM + HVF).
- **Agent Relevance**: Potential
- **Verification**: No direct AI agent usage. Supported by microvm.nix. Experimental, not officially supported by Google.
- **Status**: Experimental (Apache-2.0, Rust)
- **Include in final list**: Yes — but lower priority, notable for confidential VM support

#### Quark Container
- **Repo**: https://github.com/QuarkContainer/Quark
- **Stars**: ~345 (verified)
- **Description**: High-performance secure container runtime with co-designed guest kernel (QKernel) and VMM (QVisor).
- **Agent Relevance**: Indirect
- **Verification**: Integrated into Kuasar as app kernel sandboxer. 79.3% lower P95 latency vs Kata.
- **Status**: Active (Apache-2.0, Rust)
- **Include in final list**: Yes — novel co-designed approach with strong performance

---

### Category: MicroVM / Unikernels

#### Unikraft
- **Repo**: https://github.com/unikraft/unikraft
- **Stars**: ~3,500 (verified, research said ~3,000)
- **Description**: Cloud-native unikernel development kit. Build ultra-lightweight single-purpose VMs. Linux Foundation project.
- **Agent Relevance**: Potential
- **Verification**: No direct AI agent usage. Extreme specialization (minimal attack surface, fast boot) is conceptually ideal for agent execution.
- **Status**: Active (v0.20.0, Sep 2025. BSD-3-Clause, C)
- **Include in final list**: Yes — leading unikernel project

#### Nanos (NanoVMs)
- **Repo**: https://github.com/nanovms/nanos
- **Stars**: ~3,100 (verified)
- **Description**: Single-process unikernel with pledge/unveil sandboxing, GPU support, ~5ms boot on Firecracker.
- **Agent Relevance**: Potential
- **Verification**: No direct AI agent usage. Tagged as "sandbox" on GitHub. Single-application design inherently suited for agent isolation.
- **Status**: Active (Apache-2.0, C)
- **Include in final list**: Yes — unique pledge/unveil + GPU unikernel

#### OSv
- **Repo**: https://github.com/cloudius-systems/osv
- **Stars**: ~4,200 (verified)
- **Description**: Unikernel that runs unmodified Linux binaries. ~3-5ms boot on QEMU/Firecracker.
- **Agent Relevance**: Potential
- **Verification**: No direct AI agent usage. Runs unmodified Python/Node.js/Java which is valuable for agent workloads.
- **Status**: Community-maintained (BSD-3-Clause, C++)
- **Include in final list**: Yes — fastest-booting unikernel, runs unmodified binaries

#### IncludeOS
- **Repo**: https://github.com/includeos/IncludeOS
- **Stars**: ~5,160 (verified)
- **Description**: Minimal unikernel for C++ cloud services. OS-as-library approach with 2.5 MB minimal image.
- **Agent Relevance**: Potential
- **Verification**: No AI agent usage. C++ focus limits applicability for Python/JS agent workloads.
- **Status**: Semi-active (Apache-2.0, C++)
- **Include in final list**: Yes — notable for extreme minimalism, but limited language support

#### Hermit (RustyHermit)
- **Repo**: https://github.com/hermit-os/hermit-rs
- **Stars**: ~1,860 (verified)
- **Description**: Pure Rust unikernel for scalable cloud computing. No C/C++ code.
- **Agent Relevance**: Potential
- **Verification**: No AI agent usage. Memory-safe Rust kernel is architecturally interesting.
- **Status**: Active (Apache-2.0/MIT, Rust)
- **Include in final list**: Yes — unique pure-Rust kernel

#### MirageOS
- **Repo**: https://github.com/mirage/mirage
- **Stars**: ~2,600 (verified)
- **Description**: OCaml library OS for building unikernels. Type-safe construction for security.
- **Agent Relevance**: Potential
- **Verification**: No AI agent usage. Academic significance but limited practical agent applicability due to OCaml ecosystem.
- **Status**: Active (ISC, OCaml)
- **Include in final list**: No — too niche (OCaml-only), no practical agent relevance

#### Solo5
- **Repo**: https://github.com/Solo5/solo5
- **Stars**: ~940 (verified)
- **Description**: Sandboxed execution environment for unikernels. Multiple backends (KVM, seccomp) with only 7 allowed syscalls via spt.
- **Agent Relevance**: Potential
- **Verification**: No direct AI agent usage. Provides sandboxing layer for MirageOS. Extremely strong isolation (7 syscalls).
- **Status**: Active (ISC, C)
- **Include in final list**: Yes — notable minimal attack surface (7 syscalls)

#### kvmtool (lkvm)
- **Repo**: https://github.com/kvmtool/kvmtool
- **Stars**: ~200 (verified)
- **Description**: Minimal KVM host tool, ~5K lines of C. Educational lightweight VMM.
- **Agent Relevance**: Potential
- **Verification**: Supported by microvm.nix. Hyperfork-kvmtool adds VM flash-cloning.
- **Status**: Semi-active (GPL-2.0, C)
- **Include in final list**: No — too minimal for practical agent use, educational only

---

### Category: Kubernetes-Native Agent Sandbox

#### kubernetes-sigs/agent-sandbox
- **Repo**: https://github.com/kubernetes-sigs/agent-sandbox
- **Stars**: ~1,100 (verified, research said ~980)
- **Description**: Kubernetes CRD/controller for managing isolated AI agent runtimes. CNCF project under SIG Apps.
- **Agent Relevance**: Direct
- **Verification**: VERIFIED — launched at KubeCon NA 2025 (Google). Google Open Source Blog announcement confirmed. Uses gVisor + Kata Containers. WarmPools for sub-second startup. Documentation at agent-sandbox.sigs.k8s.io.
- **Status**: Active (v0.1.1, Feb 2026. Apache-2.0, Go)
- **Include in final list**: Yes — the Kubernetes standard for agent sandboxing

---

### Category: Container-Based — Engines & Runtimes

#### Docker (Moby)
- **Repo**: https://github.com/moby/moby
- **Stars**: ~71,500 (verified)
- **Description**: Open-source container engine that defined modern containerization.
- **Agent Relevance**: Direct
- **Verification**: Docker launched official "Docker Sandboxes" (Dec 2025) for AI coding agents. MicroVM-based isolation for Claude Code, Gemini, Codex, Kiro. No AI mentions in Moby repo itself.
- **Status**: Active (v29.2.1, Feb 2026)
- **Include in final list**: Yes — ubiquitous, now with dedicated agent sandbox feature

#### Docker Sandboxes (Official)
- **Repo**: https://docs.docker.com/ai/sandboxes (closed-source, part of Docker Desktop)
- **Stars**: N/A
- **Description**: Docker's official microVM-based sandbox for AI coding agents. Released Dec 2025 (not Nov as research stated).
- **Agent Relevance**: Direct (purpose-built)
- **Verification**: VERIFIED — Docker blog posts (Dec 3, Dec 17 2025). Supports Claude Code, Gemini CLI, Codex, Copilot, Kiro. Private Docker daemon per sandbox. Available on macOS/Windows; Linux uses container-based fallback.
- **Status**: Active (experimental, rapidly evolving)
- **Include in final list**: Yes — major industry validation of agent sandboxing

#### Podman
- **Repo**: https://github.com/containers/podman
- **Stars**: ~30,800 (verified)
- **Description**: Daemonless, rootless container engine compatible with Docker CLI. Red Hat-backed.
- **Agent Relevance**: Direct
- **Verification**: Used as Docker alternative for AI agents. Backend for llm-sandbox, yolobox, TSK, AgentBox. CNCF Sandbox project (Jan 2025). Rootless security via user namespaces.
- **Status**: Active (v5.8.0, Feb 2026)
- **Include in final list**: Yes — rootless security advantage

#### containerd
- **Repo**: https://github.com/containerd/containerd
- **Stars**: ~20,400 (verified, research said ~18,000)
- **Description**: Industry-standard container runtime. CNCF graduated. Sandbox API stable in v2.0.
- **Agent Relevance**: Indirect (foundational)
- **Verification**: Runtime underneath Docker, Kubernetes, most clouds. Sandbox API formalizes gVisor/Kata integration.
- **Status**: Active (v2.2.1, Dec 2025)
- **Include in final list**: Yes — foundational infrastructure

#### gVisor (runsc)
- **Repo**: https://github.com/google/gvisor
- **Stars**: ~17,800 (verified)
- **Description**: Application kernel implementing Linux interface in user-space Go. OCI-compatible (runsc) drop-in for runc.
- **Agent Relevance**: Direct
- **Verification**: Core technology for GKE Sandbox, kubernetes-sigs/agent-sandbox, Modal, Northflank. GPU/CUDA support. Enabled by default on GKE Autopilot. Google internally uses for Search, Gmail, Cloud Functions.
- **Status**: Active (10,712 commits, Apache-2.0, Go)
- **Include in final list**: Yes — most widely adopted container sandbox for agents

#### Sysbox
- **Repo**: https://github.com/nestybox/sysbox
- **Stars**: ~3,500 (verified)
- **Description**: Container runtime enabling rootless Docker-in-Docker and K8s-in-Docker without privileges.
- **Agent Relevance**: Direct
- **Verification**: Enables secure Docker-in-Docker for agents that build/run containers. Docker-sponsored. No explicit AI mentions in repo.
- **Status**: Active (Docker-sponsored)
- **Include in final list**: Yes — critical for agents needing nested containers

#### runc
- **Repo**: https://github.com/opencontainers/runc
- **Stars**: ~13,100 (verified)
- **Description**: OCI reference runtime. Default low-level runtime for Docker/containerd.
- **Agent Relevance**: Indirect (foundational)
- **Verification**: Shares host kernel — considered insufficient alone for untrusted AI code. Foundation everything else builds on.
- **Status**: Active (core OCI infrastructure)
- **Include in final list**: Yes — foundational reference

#### crun
- **Repo**: https://github.com/containers/crun
- **Stars**: ~3,600 (verified)
- **Description**: Fast OCI runtime in C. Default for Podman.
- **Agent Relevance**: Indirect (foundational)
- **Verification**: Faster than runc. Underlying runtime for Podman-based agent sandboxing.
- **Status**: Active (Red Hat-maintained)
- **Include in final list**: Yes — Podman's default runtime

#### youki
- **Repo**: https://github.com/youki-dev/youki
- **Stars**: ~7,200 (verified, research said ~6,400)
- **Description**: OCI runtime in Rust. Memory-safe alternative to runc.
- **Agent Relevance**: Indirect (foundational)
- **Verification**: Rust memory safety reduces runtime attack surface. CNCF sandbox project.
- **Status**: Active (v0.6.0, Feb 2026)
- **Include in final list**: Yes — Rust safety for security-critical workloads

#### LXC / Incus
- **Repo**: https://github.com/lxc/incus
- **Stars**: ~4,900 (verified)
- **Description**: System container and VM manager. Community fork of Canonical's LXD.
- **Agent Relevance**: Potential
- **Verification**: Used by "Vibebin" project for persistent AI coding agent sandboxes with SSH routing. Full OS environment isolation.
- **Status**: Active (community-led)
- **Include in final list**: Yes — viable for persistent agent environments

---

### Category: Container-Based — Agent-Specific Sandboxes

#### Daytona
- **Repo**: https://github.com/daytonaio/daytona
- **Stars**: ~60,400 (verified)
- **Description**: Secure and elastic infrastructure for running AI-generated code. Pivoted from dev environments to AI agent infra Feb 2025.
- **Agent Relevance**: Direct (purpose-built)
- **Verification**: VERIFIED — $24M Series A (Feb 2026, FirstMark). Customers: LangChain, Turing, Writer, SambaNova. $1M ARR in 3 months post-pivot. Sub-90ms sandbox creation.
- **Status**: Active (v0.146.0, Feb 2026)
- **Include in final list**: Yes — fastest-growing agent sandbox platform

#### OpenHands
- **Repo**: https://github.com/All-Hands-AI/OpenHands
- **Stars**: ~68,200 (verified, research said ~50,000+)
- **Description**: AI-powered autonomous development platform with Docker sandbox isolation. Formerly OpenDevin.
- **Agent Relevance**: Direct (purpose-built)
- **Verification**: VERIFIED — $18.8M Series A. 4M+ downloads. Contributors from AMD, Apple, Google, Amazon, Netflix, NVIDIA. Docker sandbox for all code execution. Software Agent SDK.
- **Status**: Active (v1.4.0, Feb 2026. MIT)
- **Include in final list**: Yes — largest open-source AI agent platform

#### AIO Sandbox (agent-infra)
- **Repo**: https://github.com/agent-infra/sandbox
- **Stars**: ~2,600 (verified)
- **Description**: All-in-One Docker container for AI agents with Browser, Shell, File, MCP, and VS Code Server.
- **Agent Relevance**: Direct (purpose-built)
- **Verification**: VERIFIED — Hacker News discussion, docs at sandbox.agent-infra.com. SDKs in Python/TS/Go. Apache 2.0.
- **Status**: Active (101 commits)
- **Include in final list**: Yes — comprehensive agent environment

#### Alibaba OpenSandbox
- **Repo**: https://github.com/alibaba/OpenSandbox
- **Stars**: ~1,200 (verified)
- **Description**: General-purpose sandbox platform for AI applications with multi-language SDKs and Docker/K8s runtimes.
- **Agent Relevance**: Direct (purpose-built)
- **Verification**: VERIFIED — GitHub contains explicit example directories for Claude Code, Gemini CLI, Codex CLI with READMEs. Multi-language SDKs (Java, Python, JS/TS). Apache 2.0, Alibaba-backed.
- **Status**: Active (Feb 2026 releases)
- **Include in final list**: Yes — enterprise-grade, multi-agent support

#### Dify Sandbox
- **Repo**: https://github.com/langgenius/dify-sandbox
- **Stars**: ~1,100 (verified, research said ~300)
- **Description**: Lightweight code execution environment for Dify AI platform using seccomp for syscall restriction.
- **Agent Relevance**: Direct (purpose-built)
- **Verification**: Part of Dify (~70k+ stars agentic platform). Seccomp-based syscall filtering + chroot isolation. Multi-tenant.
- **Status**: Active (v0.2.12, May 2025)
- **Include in final list**: Yes — part of major AI platform ecosystem

#### llm-sandbox
- **Repo**: https://github.com/vndee/llm-sandbox
- **Stars**: ~834 (verified)
- **Description**: Lightweight Python library for LLM code execution with Docker/Podman/K8s backends and MCP server.
- **Agent Relevance**: Direct (purpose-built)
- **Verification**: Integrates with LangChain, LlamaIndex, OpenAI, Anthropic. MCP server support. Multi-language execution.
- **Status**: Active (554 commits)
- **Include in final list**: Yes — flexible multi-backend sandbox library

#### Yolobox
- **Repo**: https://github.com/finbarr/yolobox
- **Stars**: ~451 (verified, research said ~330)
- **Description**: Run AI coding agents with full sudo in Docker/Podman containers safely.
- **Agent Relevance**: Direct (purpose-built)
- **Verification**: Explicitly for Claude Code, Codex, Gemini, OpenCode, Copilot. Homebrew installable. Created Jan 2026.
- **Status**: Active (130 commits)
- **Include in final list**: Yes — simple and practical agent sandbox

#### agent-sandbox/agent-sandbox (E2B-compatible)
- **Repo**: https://github.com/agent-sandbox/agent-sandbox
- **Stars**: ~64 (verified, research said ~200)
- **Description**: E2B-compatible, self-hosted, Kubernetes-native runtime for AI agents.
- **Agent Relevance**: Direct (purpose-built)
- **Verification**: E2B protocol compatible. MCP server. Multi-tenant. Relatively new (v0.2.0, Jan 2026).
- **Status**: Active (52 commits)
- **Include in final list**: Yes — E2B API compatibility is valuable, but low adoption

#### TSK (Task Manager & Sandbox)
- **Repo**: https://github.com/dtormoen/tsk
- **Stars**: ~200 (estimated, not independently verified)
- **Description**: Rust CLI for delegating dev tasks to AI agents in sandboxed Docker/Podman containers. Returns git branches.
- **Agent Relevance**: Direct (purpose-built)
- **Verification**: Not independently verified via WebFetch. Claims parallel task execution, network control via Squid proxy.
- **Status**: Active (claimed)
- **Include in final list**: Yes — unique git-branch-based workflow

#### Rivet Sandbox Agent SDK
- **Repo**: https://github.com/rivet-dev/sandbox-agent
- **Stars**: ~300 (estimated, not independently verified)
- **Description**: Universal HTTP API for running coding agents (Claude Code, Codex, OpenCode, Amp, Pi) in any sandbox.
- **Agent Relevance**: Direct (purpose-built)
- **Verification**: Not independently verified. Claims to solve agent API fragmentation with single HTTP API.
- **Status**: Active (claimed, open-sourced Jan 2026)
- **Include in final list**: Yes — unique universal agent API approach

#### Dify AgentBox
- **Repo**: https://github.com/langgenius/dify-agentbox
- **Stars**: ~100 (estimated, not independently verified)
- **Description**: Production-ready Docker image for AI agents with multi-language code execution.
- **Agent Relevance**: Direct (purpose-built)
- **Verification**: Part of Dify ecosystem. Multi-language (Python, Node.js, Go) + document processing.
- **Status**: Active (part of Dify)
- **Include in final list**: Yes — but lower priority, Dify ecosystem component

---

### Category: Container-Based — Agent Frameworks

#### smolagents (HuggingFace)
- **Repo**: https://github.com/huggingface/smolagents
- **Stars**: ~25,600 (verified, research said ~15,000 — significant correction)
- **Description**: Barebones library for Code Agents that think in code. Multiple sandbox backends.
- **Agent Relevance**: Direct
- **Verification**: Built-in Docker sandbox with security constraints (512MB mem, CPU quota, PID limit, no-new-privileges, dropped capabilities). Also supports E2B, Modal, Blaxel, Pyodide+Deno.
- **Status**: Active (v1.24.0, Jan 2026)
- **Include in final list**: Yes — major framework with multiple sandbox integrations

#### Open Interpreter
- **Repo**: https://github.com/OpenInterpreter/open-interpreter
- **Stars**: ~55,000+ (estimated, not independently verified)
- **Description**: Natural language code execution with experimental Docker isolation support.
- **Agent Relevance**: Direct
- **Verification**: Docker support is experimental. Most popular open-source code interpreter.
- **Status**: Active (Docker support still experimental)
- **Include in final list**: Yes — but note Docker isolation is experimental

---

### Category: Container-Based — Build Tools (Tangential)

#### Kaniko
- **Repo**: https://github.com/GoogleContainerTools/kaniko
- **Stars**: ~15,000 (estimated)
- **Description**: Build container images in containers/K8s without Docker daemon.
- **Agent Relevance**: Indirect
- **Verification**: Enables agents to build images in unprivileged environments. Google-maintained.
- **Status**: Active
- **Include in final list**: Yes — but tangential, relevant for agent workflows needing image builds

#### Buildah
- **Repo**: https://github.com/containers/buildah
- **Stars**: ~7,500 (estimated)
- **Description**: OCI image builder. Rootless, daemonless, scriptable.
- **Agent Relevance**: Indirect
- **Verification**: Rootless container image building for agent workflows.
- **Status**: Active (Red Hat-maintained)
- **Include in final list**: No — too tangential, Kaniko covers the build tool niche

---

### Category: Container-Based — System Environments

#### Apptainer (Singularity)
- **Repo**: https://github.com/apptainer/apptainer
- **Stars**: ~1,200 (estimated)
- **Description**: HPC application containers. No privilege escalation possible.
- **Agent Relevance**: Potential
- **Verification**: Strong security model. Widely used in HPC/scientific. No AI agent integrations.
- **Status**: Active (Linux Foundation project)
- **Include in final list**: Yes — relevant for HPC agent workloads

---

### Category: OS-Level Security — Process Sandboxing

#### nsjail (Google)
- **Repo**: https://github.com/google/nsjail
- **Stars**: ~3,700 (verified)
- **Description**: Lightweight process isolation using namespaces, cgroups, rlimits, and seccomp-bpf with Kafel DSL.
- **Agent Relevance**: Direct
- **Verification**: Used by Windmill for sandboxing code execution. Listed in AI sandbox curations. CTF and code execution hosting. No AI mentions in README.
- **Status**: Active (nsjail-3.4)
- **Include in final list**: Yes — battle-tested, used for code execution

#### Bubblewrap (bwrap)
- **Repo**: https://github.com/containers/bubblewrap
- **Stars**: ~5,900 (verified, research said ~5,500)
- **Description**: Low-level unprivileged sandboxing tool. Used by Flatpak.
- **Agent Relevance**: Direct
- **Verification**: VERIFIED — Used by Anthropic's Claude Code sandbox runtime on Linux. Documented in Claude Code engineering blog. Open-source @anthropic-ai/sandbox-runtime uses bwrap. Significantly faster than Docker for lightweight sandboxing.
- **Status**: Active (v0.11.0, Oct 2024)
- **Include in final list**: Yes — production-proven by Claude Code

#### Landlock LSM
- **Repo**: https://github.com/landlock-lsm (organization)
- **Stars**: rust-landlock ~201, go-landlock ~276
- **Description**: Linux kernel module for unprivileged process filesystem and network access restriction (since kernel 5.13).
- **Agent Relevance**: Direct
- **Verification**: VERIFIED for OpenAI Codex and Cursor. **CORRECTED**: Claude Code does NOT use Landlock — it uses Bubblewrap. Landlock is used by Codex (documented) and Cursor (blog post confirmed).
- **Status**: Active (kernel-maintained, ABI v5 in Linux 6.7+)
- **Include in final list**: Yes — kernel-native, used by Codex and Cursor

#### landrun
- **Repo**: https://github.com/Zouuup/landrun
- **Stars**: ~2,100 (verified, research said ~1,400)
- **Description**: Run any Linux process in a Landlock sandbox. Single binary, no root needed.
- **Agent Relevance**: Direct
- **Verification**: Explicitly designed for sandboxing untrusted binaries. Go-based CLI. Referenced in AI agent sandboxing discussions.
- **Status**: Active (91 commits)
- **Include in final list**: Yes — practical Landlock CLI wrapper

#### nono
- **Repo**: https://github.com/always-further/nono
- **Stars**: ~586 (verified)
- **Description**: Kernel-enforced sandbox CLI/SDKs for AI agents with capability-based isolation, key management, atomic rollback, and audit trails.
- **Agent Relevance**: Direct (purpose-built)
- **Verification**: VERIFIED — Hugging Face blog post, DEV Community article, Hacker News Show HN. Created by Luke Hinds (Sigstore creator). Uses Landlock (Linux) + Seatbelt (macOS). Ships profiles for Claude Code, Codex. Website: nono.sh.
- **Status**: Active (Apache 2.0, early stage)
- **Include in final list**: Yes — most feature-rich agent-specific OS-level sandbox

#### sandboxec
- **Repo**: https://github.com/dwisiswant0/sandboxec
- **Stars**: ~45 (verified, research said ~100+)
- **Description**: Lightweight single-binary Landlock-based command sandbox with MCP mode.
- **Agent Relevance**: Direct (purpose-built)
- **Verification**: VERIFIED — blog post confirms AI agent targeting, MCP mode for agent tool integration. Very new (Feb 2026), only 5 commits.
- **Status**: Active (very early, Feb 2026)
- **Include in final list**: Yes — novel MCP mode, but note very early stage and low adoption

#### Firejail
- **Repo**: https://github.com/netblue30/firejail
- **Stars**: ~7,100 (verified, research said ~6,700)
- **Description**: SUID sandbox for Linux using namespaces, seccomp-bpf, capabilities. 1000+ app profiles.
- **Agent Relevance**: Indirect
- **Verification**: Desktop application sandbox. Not purpose-built for agents but could be adapted. Landlock support (kernel 5.13+).
- **Status**: Active (v0.9.78, Jan 2026)
- **Include in final list**: Yes — mature general-purpose sandbox

#### Minijail (Google/ChromeOS)
- **Repo**: https://github.com/google/minijail
- **Stars**: ~361 (verified)
- **Description**: ChromeOS/Android sandboxing tool. Both CLI and C library for self-sandboxing.
- **Agent Relevance**: Indirect
- **Verification**: Battle-tested in Chrome OS and Android. Library API enables embedding in applications. No AI agent usage.
- **Status**: Active (Google-maintained)
- **Include in final list**: Yes — proven production sandbox technology

#### Sandboxed API / Sandbox2 (Google)
- **Repo**: https://github.com/google/sandboxed-api
- **Stars**: ~1,700 (verified)
- **Description**: Auto-generates sandboxes for C/C++ libraries. Sandbox2 is the underlying engine.
- **Agent Relevance**: Indirect
- **Verification**: Used internally at Google for production workloads. Could sandbox native code libraries called by agents.
- **Status**: Active (Google Sandbox Team)
- **Include in final list**: Yes — unique auto-sandbox generation approach

#### libseccomp
- **Repo**: https://github.com/seccomp/libseccomp
- **Stars**: ~899 (verified)
- **Description**: Platform-independent C library for Linux seccomp syscall filtering.
- **Agent Relevance**: Indirect (foundational)
- **Verification**: Used by Docker, Podman, Flatpak, Snap, Firejail. Underpins most Linux sandbox stacks.
- **Status**: Active (v2.6.0, Jan 2025)
- **Include in final list**: Yes — foundational library

#### macOS Sandbox (Seatbelt)
- **Repo**: https://github.com/s7ephen/OSX-Sandbox--Seatbelt--Profiles (community profiles)
- **Stars**: ~200+ (community repo)
- **Description**: macOS kernel-level sandboxing via sandbox-exec with Seatbelt profile language (SBPL).
- **Agent Relevance**: Direct
- **Verification**: VERIFIED — used by OpenAI Codex, Claude Code, and nono on macOS. Despite being deprecated, still functional and used by Apple internally.
- **Status**: Deprecated (but functional and actively used)
- **Include in final list**: Yes — primary macOS agent sandboxing primitive

#### AppArmor
- **Repo**: N/A (kernel LSM)
- **Stars**: N/A
- **Description**: Linux kernel MAC using path-based per-program profiles.
- **Agent Relevance**: Direct
- **Verification**: Used by CodeJail for Python sandbox enforcement. Used by Docker and Snap for container confinement. Recommended defense-in-depth layer for agent sandboxing.
- **Status**: Active (kernel-maintained)
- **Include in final list**: Yes — widely used MAC for defense-in-depth

#### CodeJail (Open edX)
- **Repo**: https://github.com/openedx/codejail
- **Stars**: ~471 (verified)
- **Description**: Untrusted Python code execution sandbox using AppArmor confinement.
- **Agent Relevance**: Direct
- **Verification**: Purpose-built for untrusted code execution. AppArmor profiles enforce isolation. Directly applicable to agent Python execution.
- **Status**: Active (v4.1.0, Nov 2025)
- **Include in final list**: Yes — proven untrusted Python execution sandbox

---

### Category: OS-Level Security — Application Distribution/Packaging (Indirect)

#### Flatpak
- **Repo**: https://github.com/flatpak/flatpak
- **Stars**: ~4,800 (verified)
- **Description**: Linux app sandboxing using bubblewrap with portal system for controlled host access.
- **Agent Relevance**: Indirect
- **Verification**: Portal pattern (user-consented resource access) is relevant to agent permission models. Desktop-focused.
- **Status**: Active (v1.16.3, Jan 2026)
- **Include in final list**: Yes — but as reference architecture for portal/permission patterns

#### Snap / snapd
- **Repo**: https://github.com/canonical/snapd
- **Stars**: ~2,000 (verified)
- **Description**: Package management with mandatory AppArmor + seccomp confinement per application.
- **Agent Relevance**: Indirect
- **Verification**: Auto-generated security profiles per snap. Interface-based permission system (plugs/slots) relevant to agent permission architecture.
- **Status**: Active (Canonical-maintained)
- **Include in final list**: Yes — but as reference for auto-generated security profiles

#### Sandstorm.io
- **Repo**: https://github.com/sandstorm-io/sandstorm
- **Stars**: ~7,000 (verified)
- **Description**: Self-hostable web platform where each document/app runs in its own containerized "grain."
- **Agent Relevance**: Indirect
- **Verification**: Per-grain sandbox pattern is relevant to per-task agent isolation. Sponsored by TestMu AI.
- **Status**: Community-maintained
- **Include in final list**: Yes — interesting per-task isolation model

#### Windows Sandbox
- **Repo**: https://github.com/microsoft/Windows-Sandbox
- **Stars**: ~512 (verified, research said ~2,000 — correction)
- **Description**: Disposable lightweight Windows environment using hypervisor-based isolation.
- **Agent Relevance**: Potential
- **Verification**: Microsoft's official sandbox. Clean state each launch. WSB config files. No AI agent usage found.
- **Status**: Active (Microsoft-maintained)
- **Include in final list**: Yes — only Windows-specific sandbox option

---

## Summary Statistics

| Metric | Count |
|--------|-------|
| Total entries evaluated | ~94 across 3 files |
| Duplicates found | 4 |
| Entries excluded | 21 |
| **Entries included in verified list** | **~65** |
| Direct agent relevance (verified) | ~30 |
| Indirect relevance | ~20 |
| Potential relevance | ~15 |
| Factual corrections made | 9 |

## Key Verification Findings

1. **All "Direct" agent relevance claims verified** for the top projects (E2B, Microsandbox, Arrakis, kubernetes-sigs/agent-sandbox, Daytona, AIO Sandbox, OpenSandbox, nono, Docker Sandboxes, OpenHands, bubblewrap/Claude Code, Landlock/Codex).

2. **One factual error corrected**: Claude Code uses Bubblewrap, NOT Landlock. Landlock is used by OpenAI Codex and Cursor.

3. **Star counts were frequently underestimated**: OpenHands (68k vs 50k claimed), smolagents (25.6k vs 15k), LiteBox (2.4k vs 330), landrun (2.1k vs 1.4k). A few were overestimated: Windows Sandbox (512 vs 2k), sandboxec (45 vs 100+).

4. **CodeDuet should be treated with extreme caution**: Only 3 stars, 15 commits, enterprise claims unverifiable. Excluded from recommended list.

5. **All repos checked exist and none are archived** (except Dragonball, which was correctly noted as archived in the research).

6. **Agent sandbox ecosystem is real and growing**: E2B ($21M Series A), Daytona ($24M Series A), OpenHands ($18.8M Series A) confirm significant VC investment in this space.
