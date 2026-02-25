# MicroVM & Hypervisor-Based Sandboxes for AI Agents

Research conducted: 2026-02-25

This document catalogs microVM, hypervisor, unikernel, and related virtualization-based sandboxing technologies relevant to AI agent isolation.

---

## 1. Firecracker

- **Repo**: https://github.com/firecracker-microvm/firecracker
- **Stars**: ~32,000
- **Description**: Secure and fast microVMs for serverless computing, developed by AWS. Minimalist VMM using KVM with <5 MiB memory overhead per VM and ~125ms boot time.
- **Agent Relevance**: Direct
- **Evidence**: Firecracker is the foundation for multiple AI agent sandbox platforms including E2B, Vercel Sandbox, CodeAnt AI, and Northflank. It powers AWS Lambda and Fargate. Widely cited as the gold standard for AI agent code execution isolation in 2025-2026.
- **Key Features**: Sub-125ms boot, <5 MiB memory per VM, up to 150 VMs/sec/host, minimal attack surface (no USB/GPU/sound emulation), rate limiters, jailer for additional host isolation, snapshot/restore support.
- **Status**: Active (Apache-2.0, Rust)

---

## 2. Cloud Hypervisor

- **Repo**: https://github.com/cloud-hypervisor/cloud-hypervisor
- **Stars**: ~5,000
- **Description**: A modern Virtual Machine Monitor for cloud workloads, written in Rust with a focus on security. Supports CPU/memory/device hotplug, Windows and Linux guests, and vhost-user device offload.
- **Agent Relevance**: Direct
- **Evidence**: Used by Arrakis (AI agent sandbox) as its VMM. Supported by Kata Containers and Kuasar for container isolation. CodeDuet built a dedicated AI agent sandbox system on Cloud Hypervisor. Default VMM recommended by Kuasar for microVM sandboxing.
- **Key Features**: Rust-based, built on rust-vmm crates, CPU/memory hotplug, Windows + Linux guests, x86-64/AArch64/RISC-V support, vhost-user, minimal footprint, shared codebase with Firecracker via rust-vmm.
- **Status**: Active (Apache-2.0/BSD-3-Clause, Rust)

---

## 3. gVisor

- **Repo**: https://github.com/google/gvisor
- **Stars**: ~17,400
- **Description**: Application kernel for containers by Google. Provides a user-space kernel written in Go that intercepts application syscalls, offering VM-like isolation without full virtualization overhead.
- **Agent Relevance**: Direct
- **Evidence**: Foundation of Google's GKE Agent Sandbox (kubernetes-sigs/agent-sandbox), a Kubernetes CRD designed specifically for AI agent code execution. gVisor is enabled by default on GKE Autopilot. Used internally at Google for Search, Gmail, and Cloud Functions. Supports CUDA/GPU for AI/ML workloads.
- **Key Features**: User-space kernel in Go, Linux-compatible syscall interception, GPU/CUDA support, warm pools for near-instant startup, OCI-compatible, no hardware virtualization required, network filtering.
- **Status**: Active (Apache-2.0, Go)

---

## 4. Kata Containers

- **Repo**: https://github.com/kata-containers/kata-containers
- **Stars**: ~7,300
- **Description**: Open source lightweight VMs that feel and perform like containers but provide hardware virtualization isolation. Supports multiple hypervisors (QEMU, Cloud Hypervisor, Firecracker).
- **Agent Relevance**: Direct
- **Evidence**: Integrated with Google's Agent Sandbox (kubernetes-sigs/agent-sandbox) at KubeCon NA 2025. Used by Northflank for AI workload isolation (2M+ microVMs/month). Supported on Microsoft AKS for pod sandboxing. Baidu uses it in production for function computing. Kuasar builds on top of Kata's VMM sandbox.
- **Key Features**: OCI-compatible, multiple VMM backends (QEMU, Cloud Hypervisor, Firecracker, Dragonball), Kubernetes-native via CRI, optimized guest kernel, Rust-based kata-agent, ttRPC communication, supports x86_64/ARM/IBM architectures.
- **Status**: Active (Apache-2.0, Rust/Go, OpenInfra Foundation)

---

## 5. QEMU/KVM

- **Repo**: https://github.com/qemu/qemu (mirror)
- **Stars**: ~10,500
- **Description**: General-purpose open-source machine emulator and virtualizer. The most feature-complete VMM, supporting full system emulation, extensive device models, and multiple architectures.
- **Agent Relevance**: Indirect
- **Evidence**: QEMU is the default hypervisor for Kata Containers and Kuasar MicroVM sandboxer. It's the baseline comparison for Firecracker. An MCP server exists (mcp-qemu-vm) for controlling QEMU VMs via LLMs. QEMU's versatility makes it useful for development/testing of agent sandboxes, but its large attack surface makes it less ideal for production agent isolation compared to minimal VMMs.
- **Key Features**: Full system emulation, extensive device support, snapshot/restore, live migration, multi-architecture (x86, ARM, RISC-V, etc.), microvm machine type for lightweight VMs, widely supported across all cloud providers.
- **Status**: Active (GPL-2.0, C)

---

## 6. Kuasar

- **Repo**: https://github.com/kuasar-io/kuasar
- **Stars**: ~1,400
- **Description**: Multi-sandbox container runtime by Huawei that supports microVM, app kernel, and Wasm sandboxes. First implementer of containerd v2.0 Sandbox API.
- **Agent Relevance**: Indirect
- **Evidence**: Supports Cloud Hypervisor, StratoVirt, Firecracker, and QEMU as VMM backends. CNCF sandbox project. 2x faster sandbox startup and 99% reduced management overhead vs Kata Containers. Enables running multiple sandbox types on a single node, which is valuable for heterogeneous agent workloads.
- **Key Features**: Multi-sandbox (microVM + app kernel + Wasm), Rust-based, containerd v2.0 Sandbox API, 1:N process model (no shim per pod), supports Cloud Hypervisor/StratoVirt/Firecracker/QEMU backends.
- **Status**: Active (Apache-2.0, Rust, CNCF Sandbox)

---

## 7. E2B

- **Repo**: https://github.com/e2b-dev/E2B
- **Stars**: ~8,900
- **Description**: Open-source secure cloud runtime for AI agents and LLM apps. Provides sandboxed cloud environments powered by Firecracker microVMs with <200ms startup and no cold starts.
- **Agent Relevance**: Direct
- **Evidence**: Purpose-built for AI agent sandboxing. Used by Manus AI, Groq, and many AI agent frameworks. Provides Python and JavaScript SDKs for agent integration. One of the most widely adopted agent sandbox platforms. E2B maintains its own Firecracker fork.
- **Key Features**: Firecracker-based, sub-200ms boot, custom sandbox templates via Dockerfile, Python/JS SDKs, filesystem and process APIs, internet access control, session persistence, code interpreter tool.
- **Status**: Active (Apache-2.0, TypeScript/Python)

---

## 8. Microsandbox

- **Repo**: https://github.com/zerocore-ai/microsandbox
- **Stars**: ~4,500
- **Description**: Open-source self-hosted sandboxes for AI agents. Combines hardware-level microVM isolation (via libkrun) with sub-200ms startup and OCI compatibility.
- **Agent Relevance**: Direct
- **Evidence**: Explicitly designed for AI agent sandboxing. Built-in MCP server for Claude and other AI tool integration. Self-hosted with no SaaS dependency. Created by Zerocore AI. Released May 2025.
- **Key Features**: libkrun-powered microVMs, sub-200ms boot, OCI-compatible, self-hosted, MCP server integration, hardware-level isolation, Rust-based, Apache-2.0 license.
- **Status**: Active (Apache-2.0, Rust)

---

## 9. Arrakis

- **Repo**: https://github.com/abshkbh/arrakis
- **Stars**: ~500+
- **Description**: Fully customizable self-hosted sandboxing solution for AI agent code execution and computer use, using Cloud Hypervisor microVMs. Features snapshot/restore for agent backtracking.
- **Agent Relevance**: Direct
- **Evidence**: Purpose-built for AI agents by Abhishek Bhardwaj (ex-Replit, Google, Microsoft, now OpenAI). Features backtracking via snapshots (useful for MCTS-based agents), VNC for computer-use agents, MCP server, REST API, and Python SDK. Cloud Hypervisor-based.
- **Key Features**: Cloud Hypervisor VMM, snapshot/restore for backtracking, VNC for GUI agents, REST API + Python SDK + MCP server, Dockerfile-based rootfs customization, overlayfs for root protection, Ubuntu guest.
- **Status**: Active (AGPL-3.0, Go)

---

## 10. libkrun

- **Repo**: https://github.com/containers/libkrun
- **Stars**: ~1,500
- **Description**: Dynamic library providing virtualization-based process isolation via KVM (Linux) and HVF (macOS/ARM64). Embeds a minimal VMM as a library with a simple C API.
- **Agent Relevance**: Indirect
- **Evidence**: Powers microsandbox (AI agent sandbox). Used by Podman for lightweight container isolation. Supports AMD SEV and Intel TDX for confidential computing. Novel Transparent Socket Impersonation (TSI) for networking. Stable 1.0 API. Preliminary AWS Nitro Enclave support.
- **Key Features**: Library-based VMM (embed in any app), KVM (Linux) + HVF (macOS), AMD SEV/Intel TDX support, TSI networking, Rust-based, stable API (SemVer), Firecracker/rust-vmm/Cloud Hypervisor derived code.
- **Status**: Active (Apache-2.0, Rust)

---

## 11. crosvm

- **Repo**: https://github.com/google/crosvm
- **Stars**: ~1,100
- **Description**: Secure, lightweight VMM written in Rust, originally developed for Chrome OS to run Linux and Android guests. Uses process-per-device sandboxing with Minijail.
- **Agent Relevance**: Potential
- **Evidence**: No direct AI agent usage found. However, crosvm's security-first design (process-per-device jailing, seccomp BPF, capability dropping) makes it relevant. Contributed to rust-vmm ecosystem. Used across Chrome OS, Android (ARCVM), and Cuttlefish. Supported by microvm.nix for NixOS MicroVMs.
- **Key Features**: Rust-based, process-per-device Minijail sandboxing, seccomp BPF filters, virtio device support, x86_64/aarch64, used in production on Chrome OS and Android.
- **Status**: Active (BSD-3-Clause, Rust, Google)

---

## 12. rust-vmm

- **Repo**: https://github.com/rust-vmm/community
- **Stars**: ~350 (vm-memory crate), varies per crate
- **Description**: Shared set of Rust virtualization crates used by Firecracker, Cloud Hypervisor, crosvm, and others. Provides reusable building blocks for VMMs (KVM bindings, virtio, memory management).
- **Agent Relevance**: Indirect
- **Evidence**: Foundation layer for most modern Rust-based VMMs used in agent sandboxing (Firecracker, Cloud Hypervisor, libkrun, Dragonball). Contributors include Amazon, Google, Intel, Red Hat, Alibaba. Enables rapid development of new VMMs for specialized agent isolation.
- **Key Features**: Modular crate ecosystem (kvm-ioctls, vm-memory, virtio-queue, etc.), shared by major VMMs, cross-hypervisor support (KVM + Microsoft Hypervisor via mshv), reference VMM implementation.
- **Status**: Active (Apache-2.0/BSD-3-Clause, Rust)

---

## 13. StratoVirt

- **Repo**: https://github.com/openeuler-mirror/stratovirt
- **Stars**: ~400
- **Description**: Enterprise-level lightweight virtualization platform by Huawei/openEuler. Supports microVM and standard VM modes, written in Rust.
- **Agent Relevance**: Indirect
- **Evidence**: Supported as a VMM backend in Kuasar for microVM sandboxing. Integrated with microvm.nix for NixOS MicroVMs. Used in Alibaba/Huawei cloud infrastructure. No direct AI agent sandbox usage found, but available as an alternative VMM backend for agent isolation platforms.
- **Key Features**: Rust-based, microVM + standard VM modes, QMP protocol, OCI compatible, libvirt management, UEFI boot, virtio-pci and VFIO passthrough, fast boot without BIOS/GRUB in microVM mode.
- **Status**: Active (Mulan PSL v2, Rust, openEuler community)

---

## 14. Dragonball Sandbox

- **Repo**: https://github.com/openanolis/dragonball-sandbox
- **Stars**: ~89
- **Description**: Collection of Rust crates by Alibaba (OpenAnolis) for building custom VMMs. Dragonball is a lightweight microVM deeply integrated into Kata Containers 3.0.
- **Agent Relevance**: Indirect
- **Evidence**: Built-in VMM for Kata Containers 3.0, optimized for container scenarios. Powers Alibaba Cloud function computing (5 billion calls/day) and elastic container instances (millions/day). Runtime and VMM in same process for optimal performance. Repository is archived but technology lives on in Kata Containers.
- **Key Features**: In-process VMM (no shim), deeply optimized for containers, async Rust runtime, used in Alibaba Cloud production at massive scale, Kata 3.0 built-in sandbox.
- **Status**: Archived (July 2023; technology continues in Kata Containers)

---

## 15. Unikraft

- **Repo**: https://github.com/unikraft/unikraft
- **Stars**: ~3,000
- **Description**: Next-generation cloud native unikernel development kit. Build highly customized and ultra-lightweight unikernel VMs. Linux Foundation / Xen Project incubator.
- **Agent Relevance**: Potential
- **Evidence**: No direct AI agent sandbox usage found. However, Unikraft's extreme specialization (minimal attack surface, fast boot, tiny footprint) makes it conceptually ideal for single-purpose agent execution environments. The companion tool KraftKit enables rapid unikernel builds.
- **Key Features**: Modular library OS, Linux-compatible, extreme customization, reduced attack surface, fast boot, small memory footprint, POSIX-compatible, multi-arch support, Linux Foundation project.
- **Status**: Active (BSD-3-Clause, C)

---

## 16. Nanos (NanoVMs)

- **Repo**: https://github.com/nanovms/nanos
- **Stars**: ~3,000
- **Description**: Kernel designed to run one and only one application in a virtualized environment. Single-process unikernel with built-in sandbox support (pledge/unveil).
- **Agent Relevance**: Potential
- **Evidence**: No direct AI agent sandbox usage found. Nanos provides pledge/unveil syscall sandboxing (from OpenBSD), GPU support via klib, and runs on Firecracker with ~5ms boot. Its single-application design is inherently suitable for isolated agent execution. Tagged as "sandbox" on GitHub.
- **Key Features**: Single-process unikernel, pledge/unveil sandboxing, GPU support, runs on QEMU/KVM/Firecracker/Xen/VMware/cloud providers, ops CLI tool, ~5ms boot on Firecracker, 15 MiB minimum memory, klib plugin system.
- **Status**: Active (Apache-2.0, C)

---

## 17. OSv

- **Repo**: https://github.com/cloudius-systems/osv
- **Stars**: ~4,200
- **Description**: Modular unikernel designed to run unmodified Linux binaries as microVMs on hypervisors. Originally by Cloudius Systems (now ScyllaDB), community-maintained.
- **Agent Relevance**: Potential
- **Evidence**: No direct AI agent usage found. OSv can boot in ~5ms on Firecracker and ~3ms on QEMU microvm, making it one of the fastest-booting unikernels. Runs unmodified Linux binaries (Python, Node.js, Java, etc.) which is valuable for agent workloads. Supports multiple hypervisors and cloud providers.
- **Key Features**: Runs unmodified Linux binaries, extremely fast boot (~3-5ms), multi-hypervisor (QEMU/KVM, Firecracker, Xen, VMware, VirtualBox, Hyperkit), musl-based libc, Capstan packaging tool, AWS/GCE/OpenStack support.
- **Status**: Community-maintained (BSD-3-Clause, C++)

---

## 18. MirageOS

- **Repo**: https://github.com/mirage/mirage
- **Stars**: ~2,600
- **Description**: Library operating system that constructs unikernels in OCaml. Targets Xen, KVM, and other backends for secure, high-performance network applications.
- **Agent Relevance**: Potential
- **Evidence**: No direct AI agent usage found. MirageOS is academically significant as a pioneering unikernel project. Its type-safe OCaml implementation provides strong correctness guarantees. Used with Solo5 for sandboxed execution. Jitsu (just-in-time summoning of unikernels) enables on-demand boot, relevant for FaaS-style agent execution.
- **Key Features**: OCaml-based library OS, type-safe by construction, Xen/KVM/Solo5 backends, extensive library ecosystem, on-demand unikernel boot (Jitsu), academic pedigree (Cambridge).
- **Status**: Active (ISC, OCaml)

---

## 19. IncludeOS

- **Repo**: https://github.com/includeos/IncludeOS
- **Stars**: ~5,160
- **Description**: Minimal, resource-efficient unikernel for C++ cloud services. Include the OS as a library with `#include <os>` and build standalone bootable services.
- **Agent Relevance**: Potential
- **Evidence**: No direct AI agent usage found. IncludeOS's extreme minimalism (2.5 MB bootable web server, ~300ms boot on QEMU/KVM, 10ms on Solo5/uKVM) makes it interesting for lightweight agent execution environments. C++ focus limits applicability for Python/JS agent workloads.
- **Key Features**: OS-as-library (#include <os>), 2.5 MB minimal image, 300ms boot, C++17 support, virtio/vmxnet3 drivers, modular TCP/IP stack, KVM/VirtualBox/VMware/cloud support.
- **Status**: Semi-active (Apache-2.0, C++)

---

## 20. Hermit (RustyHermit)

- **Repo**: https://github.com/hermit-os/hermit-rs
- **Stars**: ~1,860
- **Description**: Rust-based lightweight unikernel targeting scalable, predictable runtime for high-performance and cloud computing. Entirely written in Rust with no C/C++ code.
- **Agent Relevance**: Potential
- **Evidence**: No direct AI agent usage found. Hermit's pure-Rust implementation eliminates memory safety vulnerabilities common in C-based kernels. Originally a research project at RWTH Aachen University. Supports running Rust applications without modification.
- **Key Features**: 100% Rust kernel (no C/C++), memory/thread-safe by design, runs unmodified Rust apps, Rust toolchain integration, KVM-based, academic research origin (RWTH Aachen), supports C/C++/Go/Fortran via playground.
- **Status**: Active (Apache-2.0/MIT, Rust)

---

## 21. Solo5

- **Repo**: https://github.com/Solo5/solo5
- **Stars**: ~940
- **Description**: Sandboxed execution environment for unikernels. Provides multiple isolation backends (KVM hardware virtualization, seccomp sandboxing) with a minimal API.
- **Agent Relevance**: Potential
- **Evidence**: No direct AI agent usage found. Solo5 provides the sandboxing layer for MirageOS and other unikernels. Its spt (sandboxed process tender) uses seccomp with only 7 allowed syscalls, providing extremely strong isolation. Fast boot comparable to loading a user process, suitable for FaaS.
- **Key Features**: Multiple tenders (hvt for KVM, spt for seccomp sandbox), minimal API, fast boot, minimal attack surface, FreeBSD/OpenBSD/Linux support, ISC licensed, used by MirageOS/IncludeOS.
- **Status**: Active (ISC, C)

---

## 22. Unikernel Linux (UKL)

- **Repo**: https://github.com/unikernelLinux/ukl
- **Stars**: ~184
- **Description**: Small patch to Linux and glibc that allows building unmodified programs as unikernels linked directly into the Linux kernel, running in kernel space.
- **Agent Relevance**: Potential
- **Evidence**: No direct AI agent usage found. UKL is a research project (Boston University + Red Hat) published at EuroSys '23. By running applications in kernel space within a full Linux kernel, UKL provides unikernel performance with full Linux compatibility. Could theoretically run any Linux agent binary as a unikernel.
- **Key Features**: Unmodified Linux binary support, runs in kernel space, minimal patch to Linux + glibc, full Linux driver/feature availability, baremetal or VM boot, academic research (BU + Red Hat).
- **Status**: Research/Experimental (LGPL-2.1, C)

---

## 23. Nabla Containers

- **Repo**: https://github.com/nabla-containers (organization)
- **Stars**: Varies per repo (~200-500 across repos)
- **Description**: Containers using library OS (unikernel) techniques to minimize host kernel attack surface. Uses only 7 syscalls via seccomp, all others blocked.
- **Agent Relevance**: Potential
- **Evidence**: No direct AI agent usage found. Nabla's extreme syscall reduction (only 7 allowed syscalls) provides among the strongest isolation guarantees. Uses Solo5 as the sandboxing layer. The nabla-measurements repo provides rigorous comparison with gVisor and Kata. However, limited to nabla-specific images.
- **Key Features**: Only 7 syscalls to host kernel, library OS/unikernel approach, OCI-compatible runtime (runnc), rigorous isolation measurements, Solo5-based, experimental nabla-linux (UML-based).
- **Status**: Stale (last significant activity ~2019-2020)

---

## 24. kvmtool (lkvm)

- **Repo**: https://github.com/kvmtool/kvmtool
- **Stars**: ~200
- **Description**: Clean, lightweight KVM host tool implementation. Boots Linux guests with minimal device emulation, only ~5K lines of C code.
- **Agent Relevance**: Potential
- **Evidence**: No direct AI agent usage found. Supported by microvm.nix for NixOS MicroVMs. Its extreme simplicity (~5 KLOC) makes it useful as an educational tool and minimal VMM. Hyperfork-kvmtool fork adds VM flash-cloning, relevant for rapid agent sandbox provisioning.
- **Key Features**: ~5K lines of clean C, minimal device emulation, no BIOS dependencies, virtio devices (blk, net, 9p, balloon, console, rng), educational/learning tool, lightweight alternative to QEMU.
- **Status**: Semi-active (GPL-2.0, C)

---

## 25. Alioth

- **Repo**: https://github.com/google/alioth
- **Stars**: ~300
- **Description**: Experimental Type-2 hypervisor by Google, written from scratch in Rust. Runs on Linux (KVM) and macOS (Hypervisor.framework). Supports confidential VMs (AMD SEV/SNP).
- **Agent Relevance**: Potential
- **Evidence**: No direct AI agent usage found. Supported by microvm.nix for NixOS MicroVMs. Notable for AMD SEV/SEV-ES/SEV-SNP confidential VM support, VFIO passthrough, and cloud image boot. Used by Motor OS (microkernel for cloud). Experimental, not officially supported by Google.
- **Key Features**: Rust from scratch, KVM (Linux) + HVF (macOS), AMD SEV/SEV-ES/SEV-SNP, VFIO passthrough, virtio devices, cloud image boot, cross-platform (x86_64/aarch64).
- **Status**: Experimental (Apache-2.0, Rust)

---

## 26. Quark Container

- **Repo**: https://github.com/QuarkContainer/Quark
- **Stars**: ~345
- **Description**: High-performance secure container runtime with CRI/OCI interface. Uses a co-designed guest kernel (QKernel) and VMM (QVisor) for optimized VM-based container isolation.
- **Agent Relevance**: Indirect
- **Evidence**: Supported as the app kernel sandboxer in Kuasar. 79.3% lower P95 latency and 2.43x throughput vs Kata. 96.5% lower startup latency than cold-start mode. RDMA networking support (TSoR) for 5x throughput over Flannel. Academic paper at arXiv.
- **Key Features**: QVisor/QKernel co-design, shared memory QCall communication, RDMA/TSoR networking, Rust-based, OCI/CRI compatible, fast container startup, integrated into Kuasar as app kernel sandboxer.
- **Status**: Active (Apache-2.0, Rust)

---

## 27. Microsoft LiteBox

- **Repo**: https://github.com/microsoft/litebox
- **Stars**: ~330
- **Description**: Security-focused library OS by Microsoft, written in Rust. Reduces host attack surface by embedding OS services directly into applications. Supports kernel- and user-mode execution.
- **Agent Relevance**: Potential
- **Evidence**: Released February 2026. Explicitly mentioned in AI agent sandboxing guides as a promising middle ground between containers and VMs. Supports AMD SEV-SNP for confidential computing. Designed for running untrusted workloads. No production usage yet; experimental.
- **Key Features**: Rust library OS, minimal host interface ("North"/"South" shim architecture), AMD SEV-SNP + LVBS support, can run unmodified Linux programs on Windows, MIT licensed, Microsoft-backed.
- **Status**: Experimental (MIT, Rust)

---

## 28. Google Agent Sandbox (kubernetes-sigs)

- **Repo**: https://github.com/kubernetes-sigs/agent-sandbox
- **Stars**: ~980
- **Description**: Kubernetes CRD and controller for managing isolated, stateful AI agent runtimes. Launched at KubeCon NA 2025 as a CNCF project under SIG Apps.
- **Agent Relevance**: Direct
- **Evidence**: Purpose-built for AI agent sandboxing on Kubernetes. Uses gVisor and Kata Containers for isolation. Provides WarmPools for sub-second startup. Includes SandboxTemplate, SandboxClaim CRDs for declarative agent environment management. Integrated with GKE, designed for LangChain/ADK/agent frameworks.
- **Key Features**: Kubernetes-native CRD, gVisor + Kata Containers backends, WarmPools for fast startup, declarative API (SandboxTemplate/SandboxClaim), network isolation, workload identity, open source under kubernetes-sigs.
- **Status**: Active (Apache-2.0, Go, CNCF/Kubernetes SIG Apps)

---

## 29. microvm.nix

- **Repo**: https://github.com/microvm-nix/microvm.nix
- **Stars**: ~1,900
- **Description**: Nix Flake for building and running NixOS MicroVMs on multiple hypervisors (QEMU, Firecracker, Cloud Hypervisor, crosvm, kvmtool, StratoVirt, Alioth).
- **Agent Relevance**: Indirect
- **Evidence**: Used to sandbox AI agent platforms like OpenClaw. Provides declarative NixOS configuration for microVM management. Multi-hypervisor support makes it a flexible orchestration layer. Blog posts demonstrate using microvm.nix for sandboxing AI coding agents.
- **Key Features**: Multi-hypervisor (7 backends), Nix Flake, declarative VM management, shared directories (9p/virtiofs), tap networking with vhost-net, systemd service integration, NixOS ecosystem.
- **Status**: Active (MIT, Nix)

---

## 30. CodeDuet MicroVM AI Agent Sandbox

- **Repo**: https://github.com/CodeDuet/codeduet-microvm-ai-agent-sandbox
- **Stars**: <100
- **Description**: MicroVM AI agent sandbox system using Cloud Hypervisor with Python SDK. Supports Linux and Windows guests with hardware-level isolation.
- **Agent Relevance**: Direct
- **Evidence**: Explicitly designed as an AI agent sandbox using Cloud Hypervisor. Features include FastAPI REST server, Prometheus/Grafana monitoring, JWT/RBAC auth, multi-agent collaboration support, conversation persistence, and enterprise integration.
- **Key Features**: Cloud Hypervisor VMM, FastAPI REST API, Python SDK, Linux + Windows guests, Prometheus/Grafana observability, JWT/RBAC authentication, multi-agent support, snapshot/restore.
- **Status**: Active (Python)

---

## Summary Statistics

| Category | Count | Examples |
|----------|-------|---------|
| **Direct AI Agent Relevance** | 9 | Firecracker, gVisor, Kata Containers, E2B, Microsandbox, Arrakis, Cloud Hypervisor, Agent Sandbox (K8s), CodeDuet |
| **Indirect Relevance** | 8 | QEMU, Kuasar, libkrun, rust-vmm, StratoVirt, Dragonball, Quark, microvm.nix |
| **Potential Relevance** | 13 | Unikraft, Nanos, OSv, MirageOS, IncludeOS, Hermit, Solo5, UKL, Nabla, kvmtool, Alioth, LiteBox, crosvm |
| **Total Projects** | **30** | |

### Key Trends (2025-2026)

1. **Firecracker dominance**: Most production AI agent sandboxes use Firecracker or forks of it.
2. **Rust everywhere**: Nearly all modern VMMs are written in Rust (Firecracker, Cloud Hypervisor, libkrun, StratoVirt, Alioth, crosvm).
3. **Kubernetes convergence**: Google's Agent Sandbox CRD (gVisor + Kata) is standardizing agent isolation on Kubernetes.
4. **Self-hosted alternatives**: Microsandbox and Arrakis offer self-hosted alternatives to E2B for data-sensitive deployments.
5. **Library OS emergence**: Microsoft LiteBox represents a new approach — lighter than VMs but stronger isolation than containers.
6. **MCP integration**: Agent sandboxes increasingly ship with MCP (Model Context Protocol) servers for direct AI tool integration.
7. **Unikernels remain niche**: Despite theoretical advantages, unikernels (Unikraft, Nanos, OSv, etc.) haven't seen direct AI agent adoption yet due to limited language runtime support and ecosystem maturity.
