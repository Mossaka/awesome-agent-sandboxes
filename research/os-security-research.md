# OS-Level and Security-Focused Sandboxes for AI Agents

Research compiled on 2026-02-25.

---

## nsjail (Google)
- **Repo**: https://github.com/google/nsjail
- **Stars**: ~3,700
- **Description**: Lightweight process isolation tool using Linux namespaces, cgroups, rlimits, and seccomp-bpf syscall filters with the Kafel BPF language.
- **Agent Relevance**: Direct
- **Evidence**: Used by platforms like Windmill for sandboxing Python and Go code execution. Listed in multiple "awesome sandbox for AI" lists. Ideal for lightweight agent code execution isolation without full VM overhead. CTF and code execution hosting are primary use cases.
- **Key Features**: Protocol Buffers config, UTS/MOUNT/PID/IPC/NET/USER/CGROUPS namespaces, rlimits, seccomp-bpf via Kafel DSL, cgroups v2 support, Docker interop.
- **Status**: Active (latest release nsjail-3.4, ongoing commits)

## Firejail
- **Repo**: https://github.com/netblue30/firejail
- **Stars**: ~6,700
- **Description**: SUID sandbox program for Linux that reduces security breach risk using namespaces, seccomp-bpf, and Linux capabilities.
- **Agent Relevance**: Indirect
- **Evidence**: Primarily targets desktop application sandboxing. Could be adapted for agent isolation but not purpose-built for it. Community project focused on Linux desktop users.
- **Key Features**: 1000+ default app profiles, Landlock support (5.13+), SELinux/AppArmor compatible, written in C with virtually no dependencies, works on kernels 3.x+, network filtering, filesystem whitelisting.
- **Status**: Active (v0.9.78 released January 2026)

## Bubblewrap (bwrap)
- **Repo**: https://github.com/containers/bubblewrap
- **Stars**: ~5,500
- **Description**: Low-level unprivileged sandboxing tool used by Flatpak and similar projects, creating restricted mount namespaces.
- **Agent Relevance**: Direct
- **Evidence**: Used by Claude Code's sandbox runtime on Linux for filesystem and network isolation. Performance benchmarks show bwrap is significantly faster than Docker for lightweight sandboxing. Used with `--unshare-net` to block external communication.
- **Key Features**: User/PID/network namespace isolation, seccomp filters, no root required (uses user namespaces), minimal footprint, used as building block by Flatpak.
- **Status**: Active (regular releases, ~3-month cadence)

## Landlock LSM
- **Repo**: https://github.com/landlock-lsm (organization with multiple repos)
- **Stars**: rust-landlock ~201, go-landlock ~276, island (WIP)
- **Description**: Linux Security Module enabling unprivileged processes to restrict their own filesystem and network access rights (since kernel 5.13).
- **Agent Relevance**: Direct
- **Evidence**: Used by OpenAI Codex, Cursor, Claude Code, and nono for agent sandboxing. Kernel-native, stackable, no root required. Rapidly becoming the go-to kernel primitive for AI agent sandboxing.
- **Key Features**: Unprivileged self-restriction, filesystem access control, TCP network control (kernel 6.7+), stackable with other LSMs, Rust/Go/C libraries available.
- **Status**: Active (kernel-maintained, ABI v5 in Linux 6.7+)

## landrun
- **Repo**: https://github.com/Zouuup/landrun
- **Stars**: ~1,400
- **Description**: Run any Linux process in a secure, unprivileged sandbox using Landlock. Think firejail, but lightweight, user-friendly, and baked into the kernel.
- **Agent Relevance**: Direct
- **Evidence**: Explicitly designed for sandboxing untrusted binaries. Go-based CLI wrapping Landlock. Referenced in AI agent sandboxing discussions. Supports fine-grained file access (ro/rw/exec) and TCP port restrictions.
- **Key Features**: Single binary, no root/containers/SELinux needed, `--best-effort` mode for graceful degradation across kernel versions, fine-grained filesystem and network rules.
- **Status**: Active (recent releases, growing community)

## nono
- **Repo**: https://github.com/always-further/nono
- **Stars**: ~500+ (trending on GitHub)
- **Description**: Secure, kernel-enforced sandbox CLI and SDKs for AI agents with capability-based isolation, key management, atomic rollback, and cryptographic audit trails.
- **Agent Relevance**: Direct (purpose-built)
- **Evidence**: Explicitly built for AI agent sandboxing. Ships with profiles for Claude Code, Codex, and other coding agents. Uses Landlock on Linux, Seatbelt on macOS. Created by Luke Hinds (creator of Sigstore).
- **Key Features**: Kernel-level enforcement (Landlock/Seatbelt), atomic rollback with integrity protection, destructive command blocking, credential isolation, Merkle tree audit chain, child process restriction inheritance, Sigstore-based instruction file verification, Python/TypeScript SDKs.
- **Status**: Active (early stage, security audit ongoing, Apache 2.0)

## sandboxec
- **Repo**: https://github.com/dwisiswant0/sandboxec
- **Stars**: ~100+
- **Description**: A lightweight, single-binary command sandbox for Linux built on Landlock, secure-by-default with allow-list filesystem and TCP access controls.
- **Agent Relevance**: Direct (purpose-built)
- **Evidence**: Explicitly designed for AI agent sandboxing. Blog post titled "The most practical, fast, tiny command sandboxing for AI agents." Supports MCP mode for AI agent toolchain integration.
- **Key Features**: Single binary (Go), allow-list only, filesystem rules (r/rx/rw/rwx), TCP bind/connect rules, MCP mode for agent integration, no daemon/root/image needed.
- **Status**: Active (released February 2026)

## Minijail (Google/ChromeOS)
- **Repo**: https://github.com/google/minijail
- **Stars**: ~356 (GitHub mirror; main repo at chromium.googlesource.com)
- **Description**: Sandboxing and containment tool used in ChromeOS and Android, providing both a CLI and a C library for self-sandboxing.
- **Agent Relevance**: Indirect
- **Evidence**: Battle-tested in Chrome OS and Android. Provides both executable launcher and library API. Not specifically used for AI agents but proven sandboxing technology.
- **Key Features**: User/group ID changes, capability dropping, seccomp-BPF filtering, PID/VFS/mount namespaces, available as both CLI tool and linkable library.
- **Status**: Active (maintained by Google, chromium.googlesource.com)

## Sandboxed API / Sandbox2 (Google)
- **Repo**: https://github.com/google/sandboxed-api
- **Stars**: ~1,800+
- **Description**: Framework to automatically generate sandboxes for C/C++ libraries. Sandbox2 is the underlying low-level sandboxing engine.
- **Agent Relevance**: Indirect
- **Evidence**: Used internally at Google for production workload isolation. Sandbox2 can sandbox untrusted programs independently. Could be used to sandbox native code libraries called by agents.
- **Key Features**: Automatic sandbox generation for C/C++ libs, "sandbox once use anywhere" model, Bazel and CMake support, fine-grained policy builder, IPC communication channel.
- **Status**: Active (maintained by Google Sandbox Team)

## libseccomp
- **Repo**: https://github.com/seccomp/libseccomp
- **Stars**: ~890
- **Description**: Platform-independent C library providing an easy-to-use interface to the Linux kernel's seccomp syscall filtering mechanism.
- **Agent Relevance**: Indirect (foundational)
- **Evidence**: Foundational library used by many sandboxing tools (Docker, Podman, Flatpak, Snap, Firejail). Not used directly for agent sandboxing but underpins most Linux sandbox stacks.
- **Key Features**: Abstract BPF language into function-call API, multi-architecture support, transaction APIs, Go bindings available (libseccomp-golang).
- **Status**: Active (v2.6.0 released January 2025)

## seccompiler (rust-vmm)
- **Repo**: https://github.com/rust-vmm/seccompiler
- **Stars**: ~102
- **Description**: Rust crate providing high-level wrappers for seccomp-bpf jailing, used by Firecracker and other Rust VMMs.
- **Agent Relevance**: Indirect (foundational)
- **Evidence**: Used by Firecracker which powers E2B and other AI sandbox platforms. Provides the seccomp layer for microVM-based agent sandboxing.
- **Key Features**: BPF compilation from Rust code or JSON, allow-list approach, modular frontend/backend design, used in production by Firecracker.
- **Status**: Active (v0.5.0, part of rust-vmm ecosystem)

## pledge/unveil (Linux port by Justine Tunney)
- **Repo**: https://github.com/jart/pledge
- **Stars**: ~128
- **Description**: OpenBSD pledge() and unveil() APIs ported to Linux userspace using seccomp-BPF and Landlock LSM.
- **Agent Relevance**: Potential
- **Evidence**: Makes OpenBSD's elegant security model available on Linux. "pledge() actually makes security comprehensible" for Linux. Works on all Linux distros since RHEL6 without root.
- **Key Features**: pledge() restricts syscall categories, unveil() restricts filesystem paths via Landlock, single binary utility, no root required, works on kernels since RHEL6.
- **Status**: Maintained (part of cosmopolitan libc ecosystem)

## seccomp-pledge (SubconsciousCompute)
- **Repo**: https://github.com/SubconsciousCompute/seccomp-pledge
- **Stars**: ~9
- **Description**: Rust-based tool combining seccomp-BPF filtering with pledge/unveil sandboxing for Linux processes.
- **Agent Relevance**: Potential
- **Evidence**: Interactive and non-interactive modes for creating seccomp policies. Could be used to generate sandboxing profiles for agent processes.
- **Key Features**: Interactive policy creation, JSON serialization of syscall lists, Unix IPC socket API mode, principle of least privilege enforcement.
- **Status**: Experimental

## Landlock Make
- **Repo**: https://github.com/jart/landlock-make
- **Stars**: ~200+
- **Description**: GNU Make fork that automatically sandboxes command invocations using Landlock, restricting filesystem access to declared targets and prerequisites.
- **Agent Relevance**: Potential
- **Evidence**: Demonstrates Landlock-based build sandboxing. 5x faster than Bazel for equivalent sandboxed builds. Shows how Landlock can be integrated into build/execution tools relevant to agent workflows.
- **Key Features**: Automatic sandboxing of Make rules, strict dependency checking, ~200 lines added to GNU Make, no root/mounts/chroot/Docker needed.
- **Status**: Maintained (by Justine Tunney)

## CodeJail (Open edX)
- **Repo**: https://github.com/openedx/codejail
- **Stars**: ~465
- **Description**: Manages execution of untrusted code in secure sandboxes using AppArmor confinement, designed primarily for Python.
- **Agent Relevance**: Direct
- **Evidence**: Purpose-built for executing untrusted Python code in educational contexts. Security enforced via AppArmor profiles. Directly applicable to AI agent Python code execution sandboxing.
- **Key Features**: AppArmor-based isolation, Python-focused, virtualenv per sandbox, resource limits, Tutor plugin for containerized deployment.
- **Status**: Active (v3.5.2 with Ubuntu 24.04 support)

## Flatpak
- **Repo**: https://github.com/flatpak/flatpak
- **Stars**: ~4,800
- **Description**: Linux application sandboxing and distribution framework using bubblewrap for namespace isolation with a portal mechanism for controlled host access.
- **Agent Relevance**: Indirect
- **Evidence**: Desktop application sandbox with sophisticated portal mechanism for mediated host access. Portal pattern (user-consented resource access) is relevant to agent permission models.
- **Key Features**: bubblewrap-based isolation, portal system (file chooser, D-Bus, etc.) for mediated access, FUSE-based document portal, GTK3/Qt5 integration, seccomp filters, per-app sandboxes.
- **Status**: Active (major Linux desktop ecosystem component)

## Snap / snapd (Canonical)
- **Repo**: https://github.com/canonical/snapd
- **Stars**: ~2,000
- **Description**: Package management system with mandatory confinement using AppArmor profiles and seccomp-BPF filters per application.
- **Agent Relevance**: Indirect
- **Evidence**: Demonstrates production-scale mandatory AppArmor + seccomp confinement. Auto-generated security profiles per snap. Relevant architecture for per-agent security policy generation.
- **Key Features**: Auto-generated AppArmor profiles and seccomp filters per command, device cgroups, strict/classic/devmode confinement levels, interface-based permission system (plugs/slots).
- **Status**: Active (Canonical-maintained, core Ubuntu component)

## Chromium Sandbox (Linux)
- **Repo**: https://chromium.googlesource.com/chromium/src/+/refs/heads/lkgr/sandbox/linux/
- **Stars**: N/A (part of Chromium monorepo)
- **Description**: Multi-layer sandbox using setuid/user namespaces (Layer 1, process confinement) and seccomp-BPF (Layer 2, attack surface reduction).
- **Agent Relevance**: Indirect
- **Evidence**: The most battle-tested process sandbox in the world (billions of Chrome users). Two-layer design is an influential reference architecture. seccomp-BPF was largely developed for/by Chromium.
- **Key Features**: Two-layer architecture (namespaces + seccomp-BPF), namespace sandbox replacing setuid sandbox, per-process BPF programs, broker process pattern for mediated access.
- **Status**: Active (core Chromium component)

## Capsicum (FreeBSD)
- **Repo**: N/A (FreeBSD kernel, Cambridge Computer Lab)
- **Stars**: N/A (kernel feature)
- **Description**: Lightweight OS capability framework extending POSIX with capability mode (process sandboxes), capabilities (refined file descriptors with fine-grained rights), and process descriptors.
- **Agent Relevance**: Potential
- **Evidence**: Influential capability-based security model. Treats file descriptors as capabilities. Used in FreeBSD base system (tcpdump, dhclient, etc.). Relevant design patterns for capability-based agent sandboxing.
- **Key Features**: cap_enter() for sandbox entry, fine-grained descriptor rights (CAP_READ, CAP_BIND, etc.), Casper daemon for delegated operations, oblivious sandboxing via capsh/libpreopen, debugging with SIGTRAP.
- **Status**: Active (FreeBSD kernel feature since 9.0, default since 10.0)

## macOS Sandbox (sandbox-exec / Seatbelt)
- **Repo**: https://github.com/s7ephen/OSX-Sandbox--Seatbelt--Profiles (community profiles)
- **Stars**: ~200+ (community repo)
- **Description**: macOS kernel-level sandboxing via sandbox-exec using Seatbelt profile language (SBPL) for fine-grained process restrictions.
- **Agent Relevance**: Direct
- **Evidence**: Used by OpenAI Codex, Claude Code, Qwen Code, and nono for agent sandboxing on macOS. Despite being deprecated, still functional and used by Apple internally. Core sandboxing primitive for macOS-based AI coding agents.
- **Key Features**: Kernel-level enforcement, Seatbelt profile language (SBPL), file/network/process restrictions, used by macOS App Sandbox, profiles at /System/Library/Sandbox/Profiles.
- **Status**: Deprecated (but functional and still used by Apple and AI tools)

## Windows Sandbox
- **Repo**: https://github.com/microsoft/Windows-Sandbox
- **Stars**: ~2,000+
- **Description**: Disposable, secure, and lightweight Windows desktop environment using hypervisor-based isolation for running applications in isolation.
- **Agent Relevance**: Potential
- **Evidence**: Microsoft's official lightweight VM sandbox. Uses hardware-based virtualization for kernel isolation. Could be used for Windows-based agent code execution. Nothing persists after close.
- **Key Features**: Hypervisor-based kernel isolation, disposable (clean state each launch), reboot persistence (22H2+), WSB configuration files, shared folders, included in Pro/Enterprise/Education SKUs.
- **Status**: Active (Microsoft-maintained, Windows feature)

## HP Sure Click / Bromium
- **Repo**: N/A (proprietary, acquired by HP in 2019)
- **Stars**: N/A
- **Description**: Micro-virtualization technology that creates lightweight, task-specific micro-VMs for each untrusted application, link click, or document download.
- **Agent Relevance**: Indirect
- **Evidence**: Pioneered micro-virtualization concept using Xen-based "Microvisor." Each process gets its own micro-VM, destroyed when process stops. Hardware-enforced isolation model relevant to agent sandboxing architecture.
- **Key Features**: Xen-based Microvisor, per-task micro-VMs, hardware-enforced isolation (VT-x), automatic micro-VM creation/destruction, zero persistence.
- **Status**: Active (as HP Sure Click Enterprise, proprietary)

## Sandstorm.io
- **Repo**: https://github.com/sandstorm-io/sandstorm
- **Stars**: ~7,000
- **Description**: Self-hostable web productivity suite implemented as a security-hardened web app package manager where each document/app runs in its own containerized "grain."
- **Agent Relevance**: Indirect
- **Evidence**: Each "grain" (document, app instance) runs in its own secure sandbox with no external access unless explicitly granted. Pattern of per-task sandboxed environments is relevant to agent architecture. Claims 95% of security vulnerabilities automatically mitigated.
- **Key Features**: Per-grain containerized sandboxes, capability-based security, no external access by default, sharing via explicit grants, self-hostable on x86-64 Linux.
- **Status**: Community-maintained (sponsored by TestMu AI)

## Mbox (MIT PDOS)
- **Repo**: https://github.com/tsgates/mbox
- **Stars**: ~668
- **Description**: Lightweight sandboxing mechanism using ptrace and seccomp/BPF that intercepts filesystem modifications, providing a layered sandbox filesystem with selective commit.
- **Agent Relevance**: Potential
- **Evidence**: No-privilege sandbox with filesystem overlay. Lets user review and selectively commit changes made by sandboxed process. Revision-control-like model relevant to agent file modification sandboxing.
- **Key Features**: No root required, layered sandbox filesystem, ptrace + seccomp/BPF interposition, selective commit of changes back to host, network connection logging, USENIX ATC '13 paper.
- **Status**: Stale (last significant updates several years ago, academic project)

## PRoot
- **Repo**: https://github.com/proot-me/PRoot
- **Stars**: ~2,400
- **Description**: ptrace-based tool providing chroot, mount --bind, and binfmt_misc without privileges, enabling filesystem path translation for sandboxed processes.
- **Agent Relevance**: Indirect
- **Evidence**: Enables unprivileged chroot-like environments. Used by udocker as execution engine. Performance penalty from ptrace makes it less suitable for high-performance agent workloads.
- **Key Features**: No root/setup required, ptrace-based syscall interception, filesystem path translation, binfmt_misc support, works on any Linux kernel.
- **Status**: Maintained (also has Rust rewrite: proot-rs with ~151 stars)

## udocker
- **Repo**: https://github.com/indigo-dc/udocker
- **Stars**: ~1,700
- **Description**: User tool to execute Docker containers in batch or interactive systems without root privileges, using PRoot for root emulation.
- **Agent Relevance**: Indirect
- **Evidence**: Enables running Docker containers without root, useful in HPC/batch environments. Does NOT offer robust isolation -- unsuitable for untrusted agent code. Good for convenience, not security.
- **Key Features**: No root required, PRoot-based execution, Docker image support, HPC/batch system compatible, multiple execution engines (PRoot, Fakechroot, runc).
- **Status**: Active (academic/research tool)

## RootlessKit
- **Repo**: https://github.com/rootless-containers/rootlesskit
- **Stars**: ~1,200
- **Description**: Linux-native "fake root" using user namespaces for implementing rootless containers, used by Docker and Kubernetes rootless modes.
- **Agent Relevance**: Indirect (foundational)
- **Evidence**: Enables rootless Docker/Kubernetes which are used for agent sandboxing. Provides the user namespace foundation for rootless container execution.
- **Key Features**: User/mount namespace creation, newuidmap/newgidmap integration, network namespace isolation via slirp4netns/vpnkit, sandbox hardening options for slirp.
- **Status**: Active (v3.0.0 alpha, used by Docker/Podman/Buildah/k3s)

## slirp4netns
- **Repo**: https://github.com/rootless-containers/slirp4netns
- **Stars**: ~700+
- **Description**: User-mode networking for unprivileged network namespaces, connecting TAP devices to usermode TCP/IP stack.
- **Agent Relevance**: Indirect (foundational)
- **Evidence**: Enables network namespace isolation without root. Used by Podman, Docker, k3s for rootless networking. Essential infrastructure for rootless container-based agent sandboxing.
- **Key Features**: Unprivileged network namespace connectivity, statically linked binaries for multiple architectures, mount namespace sandboxing for slirp process, seccomp hardening option.
- **Status**: Active (v1.3.3, used by major container tools)

## fakechroot
- **Repo**: https://github.com/dex4er/fakechroot
- **Stars**: ~200+
- **Description**: Provides a fake chroot environment using LD_PRELOAD to intercept filesystem operations, enabling chroot-like behavior without root.
- **Agent Relevance**: Indirect
- **Evidence**: LD_PRELOAD-based approach is lightweight but not security-focused. Can be combined with fakeroot for unprivileged package installation. Not suitable for security sandboxing of untrusted agent code.
- **Key Features**: LD_PRELOAD filesystem interception, no root required, works with fakeroot and debootstrap.
- **Status**: Maintained (but limited security guarantees)

## sandboxlib (CodethinkLabs)
- **Repo**: https://github.com/CodethinkLabs/sandboxlib
- **Stars**: ~8
- **Description**: Python sandboxing library treating sandboxes as lightweight temporary environments, with backends including bubblewrap.
- **Agent Relevance**: Potential
- **Evidence**: Python API for sandbox creation. Bubblewrap backend. Designed for build isolation but could be adapted for agent code execution. Lightweight and dynamic sandbox creation model.
- **Key Features**: Python API, bubblewrap backend, dynamic sandbox creation, build isolation focus, run-sandbox CLI tool.
- **Status**: Stale (last updated December 2022)

## SELinux Sandbox
- **Repo**: N/A (part of SELinux/policycoreutils)
- **Stars**: N/A
- **Description**: SELinux-based sandbox using Mandatory Access Control with `sandbox_t` domain to confine processes, preventing file creation/opening, network access, and cross-process interference.
- **Agent Relevance**: Indirect
- **Evidence**: Production-grade MAC for container isolation. MCS (Multi-Category Security) labels provide per-container isolation. Used with Docker/Podman for container security. Complex configuration limits direct agent use.
- **Key Features**: Mandatory Access Control, sandbox_t domain (no file open/create, no network), sandbox_x_t for X11 isolation with Xephyr, MCS labels for container separation, custom policy types.
- **Status**: Active (Red Hat/Fedora maintained, core Linux security feature)

## AppArmor (sandbox profiles)
- **Repo**: N/A (kernel LSM, profiles per-application)
- **Stars**: N/A
- **Description**: Linux kernel MAC system using path-based per-program profiles to restrict filesystem, network, and capability access.
- **Agent Relevance**: Direct
- **Evidence**: Used by CodeJail for Python code execution sandboxing. Used by Snap and Docker for container confinement. Defense-in-depth layer recommended for AI agent sandboxing alongside containers.
- **Key Features**: Path-based MAC, per-program profiles, enforcement and complain modes, capability restrictions, network access control, integrated with Docker/Snap/LXC.
- **Status**: Active (kernel-maintained LSM)

---

## Summary Statistics

| Category | Count |
|----------|-------|
| **Total projects researched** | **31** |
| Direct agent relevance | 11 |
| Indirect relevance | 14 |
| Potential relevance | 6 |
| Purpose-built for AI agents | 3 (nono, sandboxec, CodeJail) |
| Active maintenance | 25 |
| Stale/archived | 2 |
| Kernel-native (no extra runtime) | 8 |

## Key Findings

1. **Landlock is the new hotness**: Landlock LSM (kernel 5.13+) has emerged as the preferred kernel primitive for AI agent sandboxing. Projects like landrun, nono, sandboxec, and Cursor all build on it. It enables unprivileged, rootless sandboxing with no container overhead.

2. **Layered security is the consensus**: The industry recommends defense-in-depth -- seccomp + Landlock/AppArmor + namespaces. Chromium's two-layer model (namespaces for confinement + seccomp for attack surface reduction) is the gold standard reference architecture.

3. **Agent-specific tools are emerging**: nono, sandboxec, and landrun are purpose-built or specifically marketed for AI agent sandboxing, with features like MCP integration, audit trails, and credential isolation.

4. **Claude Code uses bubblewrap**: Anthropic's Claude Code uses bubblewrap (bwrap) on Linux and Seatbelt on macOS for its sandbox runtime, demonstrating the viability of lightweight OS-level sandboxing for production AI agents.

5. **macOS Seatbelt lives on**: Despite being deprecated, sandbox-exec/Seatbelt remains the primary macOS sandboxing primitive, used by Codex, Claude Code, and Qwen Code.

6. **Traditional tools (firejail, nsjail) still relevant**: These battle-tested tools remain viable for agent sandboxing, especially nsjail which is already used in production for code execution platforms.
