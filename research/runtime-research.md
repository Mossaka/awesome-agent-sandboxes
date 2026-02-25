# Language-Runtime & Novel Sandboxing Approaches for AI Agents

Research compiled for the agent-sandboxes project. Covers language-runtime sandboxes,
code execution engines, configuration languages with safety guarantees, TEE/eBPF
approaches, and academic research on sandboxing for AI agents.

---

## JavaScript / TypeScript Runtimes

### Deno
- **Repo**: https://github.com/denoland/deno
- **Stars**: ~106,000
- **Description**: A modern runtime for JavaScript and TypeScript with a built-in permissions-based security model; no I/O access by default.
- **Agent Relevance**: Direct
- **Evidence**: Deno launched "Deno Sandbox" — purpose-built Firecracker microVMs for running AI-generated code. LangChain Sandbox, Pydantic's MCP server, and LMStudio all use Deno for sandboxed AI code execution. Granular permission flags (`--allow-read`, `--allow-net`), permission broker, and audit logging.
- **Key Features**: Permission flags with fine-grained paths/hosts, permission broker for policy delegation, Deno Sandbox cloud product (Firecracker microVMs, <1s boot, secret injection, network filtering), TypeScript first-class support.
- **Status**: Active (very actively maintained, v2.6.x)

### Deno Sandbox (Cloud Product)
- **Repo**: https://github.com/denoland/deno (product docs at https://docs.deno.com/sandbox/)
- **Stars**: N/A (cloud product)
- **Description**: Lightweight Firecracker microVMs for running untrusted code with defense-in-depth security, secret management, and network filtering.
- **Agent Relevance**: Direct
- **Evidence**: Explicitly designed for AI agents. Secrets replaced with placeholder tokens; real values injected only at network layer to approved destinations. Used by LangChain, Pydantic AI, and LMStudio.
- **Key Features**: Sub-second boot, per-sandbox filesystem/network/process isolation, configurable allowNet, secret management, JS/Python SDKs.
- **Status**: Active (production product)

### Bun
- **Repo**: https://github.com/oven-sh/bun
- **Stars**: ~78,000
- **Description**: Fast all-in-one JavaScript runtime (bundler, transpiler, package manager). Acquired by Anthropic in 2025/2026.
- **Agent Relevance**: Potential (evolving toward Direct)
- **Evidence**: GitHub Issue #25929 proposes Deno-compatible permissions model for AI agent sandboxing. Anthropic's acquisition signals intent to make Bun an "AI-native runtime." No built-in sandbox yet, but active development.
- **Key Features**: Fastest JS startup/execution, built-in bundler/test runner, `Bun.secrets` for credential management. Permissions sandbox in development.
- **Status**: Active (very actively maintained, now owned by Anthropic)

### Val Town
- **Repo**: N/A (platform at https://www.val.town/)
- **Stars**: N/A (SaaS platform)
- **Description**: Collaborative serverless JavaScript/TypeScript platform — write, run, and deploy code in the browser with instant execution.
- **Agent Relevance**: Indirect
- **Evidence**: Used via integrations (e.g., Dust) for AI agent function creation and code execution. MCP server available. Not primarily designed as a sandbox but provides isolated execution.
- **Key Features**: Instant deployment, standard library for storage/email/AI, MCP server, CLI/IDE integration.
- **Status**: Active

---

## QuickJS-based Sandboxes

### quickjs-emscripten
- **Repo**: https://github.com/justjake/quickjs-emscripten
- **Stars**: ~1,500
- **Description**: Safely execute untrusted JavaScript in your JavaScript via QuickJS compiled to WebAssembly. Synchronous and async execution support.
- **Agent Relevance**: Direct
- **Evidence**: Used by Simon Willison and others for sandboxed execution of untrusted JS in browser/Node. Foundation for higher-level sandbox wrappers. 112k+ weekly npm downloads.
- **Key Features**: WebAssembly isolation, sync/async variants, Cloudflare Workers support, ES2020+ compatibility, memory-safe boundary between host and guest.
- **Status**: Active (maintenance mode but supported)

### @sebastianwessel/quickjs
- **Repo**: https://github.com/sebastianwessel/quickjs
- **Stars**: ~805
- **Description**: TypeScript package to execute JavaScript and TypeScript code in a WebAssembly QuickJS sandbox, with Node.js-like module support and virtual filesystem.
- **Agent Relevance**: Direct
- **Evidence**: Explicitly designed for LLM tool calling and AI-generated code execution. Has a dedicated MCP server (QuickJS Sandbox MCP). Blog posts detail AI agent integration patterns.
- **Key Features**: TypeScript support, virtual filesystem, Node.js-like modules (fs, path, util), fetch API (with security caveats), MCP server integration.
- **Status**: Active (v3.0)

---

## V8 Isolate-based Sandboxes

### isolated-vm
- **Repo**: https://github.com/laverdet/isolated-vm
- **Stars**: ~2,700
- **Description**: Access V8's Isolate interface from Node.js — create completely isolated JavaScript environments with separate heaps.
- **Agent Relevance**: Indirect
- **Evidence**: Recommended replacement for deprecated vm2. Used by Temporal SDK for deterministic workflow isolation, Screeps for running untrusted player code, and Cloudflare-style edge computing. Real V8 isolate separation (not just different global objects like node:vm).
- **Key Features**: True V8 isolate separation, cross-isolate data marshalling, multi-threaded execution, memory limits, CPU time limits.
- **Status**: Active (maintenance mode, v6.0.2, Oct 2025)

---

## Python Sandboxes

### Pyodide
- **Repo**: https://github.com/pyodide/pyodide
- **Stars**: ~14,200
- **Description**: CPython distribution compiled to WebAssembly for browser and Node.js, with full Python ⟺ JavaScript FFI.
- **Agent Relevance**: Direct
- **Evidence**: Foundation for LangChain Sandbox, Cohere Terrarium, Pydantic's mcp-run-python. NVIDIA blog recommends Pyodide for sandboxing agentic AI workflows. Widely adopted as the standard approach for sandboxed Python execution in AI agents.
- **Key Features**: Full CPython compatibility (3.12+), 100+ packages (numpy, pandas, matplotlib), virtual filesystem, JavaScript interop, browser and server-side execution.
- **Status**: Active (v0.29.3, very actively maintained)

### LangChain Sandbox
- **Repo**: https://github.com/langchain-ai/langchain-sandbox
- **Stars**: ~234 (archived)
- **Description**: Safely run untrusted Python code using Pyodide and Deno, with configurable permissions and session-based state persistence.
- **Agent Relevance**: Direct
- **Evidence**: Purpose-built by LangChain for AI agent code execution. Wraps Pyodide in Deno subprocess for double isolation. No Docker required.
- **Key Features**: Pyodide + Deno double isolation, session state persistence, configurable permissions, pip-installable.
- **Status**: Archived (superseded by newer approaches)

### Cohere Terrarium
- **Repo**: https://github.com/cohere-ai/cohere-terrarium
- **Stars**: ~263
- **Description**: A simple Python sandbox for LLM data agents — executes Python inside Node.js via Pyodide (CPython compiled to WebAssembly).
- **Agent Relevance**: Direct
- **Evidence**: Purpose-built by Cohere for LLM data agents. No filesystem access, no network access, full environment recycled after every call. Supports numpy, pandas, matplotlib.
- **Key Features**: No filesystem/network access, memory-only guest filesystem, file I/O via request/response, Docker deployment, GCP Cloud Run support.
- **Status**: Stale (last significant activity 2023-2024)

### Pydantic Monty
- **Repo**: https://github.com/pydantic/monty
- **Stars**: ~5,670
- **Description**: A minimal, secure Python interpreter written in Rust for AI — executes a Python subset with microsecond startup times.
- **Agent Relevance**: Direct
- **Evidence**: Designed specifically for AI agent code execution. Will power code-mode in Pydantic AI. LLM writes Python that calls tools as functions; Monty executes safely. Compiles to WebAssembly.
- **Key Features**: Single-digit microsecond startup, bytecode VM, no filesystem/network/env access by default, host function callbacks, state snapshotting, WebAssembly compilation.
- **Status**: Active (v0.0.7, Feb 2026, rapidly developing)

### Pydantic mcp-run-python
- **Repo**: https://github.com/pydantic/mcp-run-python
- **Stars**: ~1,200 (estimated)
- **Description**: MCP server to run Python code in a sandbox using Pyodide in Deno.
- **Agent Relevance**: Direct
- **Evidence**: Directly designed for AI agents via MCP protocol. Code executed using Pyodide in Deno for isolation. Caveat: Python in Pyodide can run arbitrary JavaScript.
- **Key Features**: MCP protocol integration, Pyodide + Deno isolation, seamless Claude/AI agent integration.
- **Status**: Active

### RestrictedPython
- **Repo**: https://github.com/zopefoundation/RestrictedPython
- **Stars**: ~628
- **Description**: A restricted execution environment for Python to run untrusted code — compilation-level restriction tool (not a full sandbox).
- **Agent Relevance**: Indirect
- **Evidence**: Used in some Python applications for limiting what untrusted code can do, but explicitly "not a sandbox." Multiple CVEs for sandbox escape (stack frame escape, format string escape, try/except* escape). Must be combined with other security measures.
- **Key Features**: Compilation-level restrictions, configurable policy, CPython 3.9-3.13 support. Whitelisting approach for builtins.
- **Status**: Active (v8.2, actively maintained)

### PyPy Sandbox
- **Repo**: https://github.com/pypy/pypy (sandbox branches: sandbox-2, py3.6-sandbox-2)
- **Stars**: ~950 (sandbox-specific)
- **Description**: PyPy's sandbox mode replaces all external function calls with stubs that communicate with an external controller process — achieving OS-level sandboxing portably.
- **Agent Relevance**: Potential
- **Evidence**: Novel approach: untrusted code runs in a subprocess where ALL external calls are serialized to stdin/stdout. Controller process decides what's allowed. Unlike RestrictedPython, doesn't try to limit language features. Prototype only — needs work for completeness.
- **Key Features**: All external calls intercepted, controller-based policy, virtual filesystem, heap size limits, CPU time limits, no language feature restrictions.
- **Status**: Stale (prototype, new version in progress but incomplete)

### RustPython
- **Repo**: https://github.com/RustPython/RustPython
- **Stars**: ~21,800
- **Description**: A Python interpreter written in Rust — can be embedded in Rust apps or compiled to WebAssembly for browser execution.
- **Agent Relevance**: Potential
- **Evidence**: Pure Rust implementation means full control over interpreter capabilities. Can compile to WebAssembly for strong isolation. Used by GreptimeDB for embedded scripting. Not production-ready but appealing for sandboxing due to control over what interpreter can access.
- **Key Features**: Pure Rust (memory-safe), WebAssembly compilation, embeddable in Rust applications, CPython 3.x compatibility (partial).
- **Status**: Active (in development, not production-ready)

### CodeJail (OpenEdx)
- **Repo**: https://github.com/openedx/codejail
- **Stars**: ~80 (estimated)
- **Description**: Manages execution of untrusted code in secure sandboxes using AppArmor, designed primarily for Python but supports other languages.
- **Agent Relevance**: Indirect
- **Evidence**: Production-tested at scale for educational platform (edX/OpenEdx). Uses AppArmor for OS-level enforcement rather than language-level restrictions. Proven approach for running student-submitted code.
- **Key Features**: AppArmor enforcement, Python-focused, process isolation, configurable resource limits.
- **Status**: Active (maintained by OpenEdx)

---

## Code Execution Engines

### Piston
- **Repo**: https://github.com/engineer-man/piston
- **Stars**: ~2,528
- **Description**: A high-performance general-purpose code execution engine using Linux namespaces, chroot, cgroups, and multiple unprivileged users inside Docker.
- **Agent Relevance**: Direct
- **Evidence**: MCP server available (alvii147/piston-mcp) for LLM code execution. Supports 50+ languages. Used by 4,100+ Discord servers for code evaluation. API recently restricted to non-commercial use.
- **Key Features**: 50+ languages, Linux namespaces/chroot/cgroups, per-execution isolation, configurable resource limits, REST API.
- **Status**: Active (but public API restricted as of Feb 2026)

### Judge0
- **Repo**: https://github.com/judge0/judge0
- **Stars**: ~3,700
- **Description**: Robust, scalable, open-source online code execution system for humans and AI — supports 60+ languages with sandboxed execution.
- **Agent Relevance**: Direct
- **Evidence**: Explicitly markets itself for AI agent code execution. MCP server available. Used by Kestra for LLM chat completion with code execution. Custom fork (Agent-01/judge0-custom) exists for enhanced agent support.
- **Key Features**: 60+ languages, Docker + Isolate sandboxing, REST API, HTTP callbacks, multi-file programs, configurable time/memory limits, Standard and Extra editions.
- **Status**: Active (v1.12.0, actively maintained)

### Compiler Explorer (Godbolt)
- **Repo**: https://github.com/compiler-explorer/compiler-explorer
- **Stars**: ~18,600
- **Description**: Run compilers interactively from your web browser and interact with the assembly output. Serves 3M+ compilations/week.
- **Agent Relevance**: Indirect
- **Evidence**: Not designed for AI agents, but provides infrastructure for safe compilation and execution of code. Could be used as a backend for code analysis agents.
- **Key Features**: 30+ languages, assembly visualization, diff mode, compiler optimization exploration, open API.
- **Status**: Active (very actively maintained since 2012)

### Wandbox
- **Repo**: https://github.com/melpon/wandbox
- **Stars**: ~1,100 (estimated)
- **Description**: Social compilation service supporting 20+ languages including C++, Python, Ruby, Rust, Haskell, and more.
- **Agent Relevance**: Indirect
- **Evidence**: Open API available for programmatic code compilation/execution. Community wrappers for Python, Vim, Emacs, VSCode. Not designed for AI but usable as execution backend.
- **Key Features**: 20+ languages, REST API, social sharing, multiple compiler versions per language.
- **Status**: Active (wandbox.org)

### Glot
- **Repo**: https://github.com/glotcode/glot
- **Stars**: ~115 (code-runner)
- **Description**: Open-source code playground using Docker containers for isolation. Reads code as JSON from stdin, compiles/runs, outputs JSON.
- **Agent Relevance**: Indirect
- **Evidence**: Architecture (JSON in → compile/run → JSON out) is ideal for programmatic code execution. Docker-based isolation. Original version archived; newer version under glotcode org.
- **Key Features**: Docker isolation, JSON-based I/O, multiple language support, simple architecture.
- **Status**: Active (glotcode org) / Archived (original prasmussen version)

### Sphere Engine
- **Repo**: N/A (commercial product at https://sphere-engine.com)
- **Stars**: N/A (proprietary)
- **Description**: Commercial APIs and software components for code execution in secure sandboxed environments — compilers API and containers API.
- **Agent Relevance**: Indirect
- **Evidence**: Supports isolated sandbox execution with per-stage isolation. Not open source but provides REST APIs usable by AI agents. Primarily targets assessment/education platforms.
- **Key Features**: 60+ languages, isolated/fast execution modes, REST API, JavaScript widget, multi-file support.
- **Status**: Active (commercial product)

---

## Embedded Scripting Languages (Sandboxed by Design)

### Rhai
- **Repo**: https://github.com/rhaiscript/rhai
- **Stars**: ~5,100
- **Description**: An embedded scripting language for Rust — safe, sandboxed by default, with tight Rust integration and a "Don't Panic" guarantee.
- **Agent Relevance**: Potential
- **Evidence**: Sandboxed by design — scripts cannot read outside their environment. Immutable engine instances are re-entrant. JavaScript+Rust-like syntax. Not specifically targeted at AI agents but ideal for embedding safe scripting in Rust-based agent frameworks.
- **Key Features**: Memory-safe sandbox by default, Don't Panic guarantee, Rust type integration, plugin system, module system, WASM compilation, debugging interface.
- **Status**: Active (4.7M+ crate downloads)

### Lune (Luau Runtime)
- **Repo**: https://github.com/lune-org/lune
- **Stars**: ~827
- **Description**: A standalone Luau runtime built in Rust — fully async APIs, filesystem/networking/stdio built-in, with VM-level sandboxing and codegen.
- **Agent Relevance**: Potential
- **Evidence**: Luau (Roblox's Lua variant) is designed to be safe against actively malicious code. VM sandboxing protects against prototype chain attacks. Built in Rust for safety. Not specifically for AI agents.
- **Key Features**: Luau VM sandboxing, codegen for 2x performance, async APIs, compile to standalone executables, Rust-built safety.
- **Status**: Active (v0.10.4)

### Lua Sandbox (kikito/lua-sandbox)
- **Repo**: https://github.com/kikito/lua-sandbox
- **Stars**: ~300 (estimated)
- **Description**: A pure-Lua solution for running untrusted Lua code — restricts dangerous functions, prevents infinite loops via instruction counting.
- **Agent Relevance**: Potential
- **Evidence**: Used by Kong (fork: Kong/kong-lua-sandbox) in production. Simple API for sandboxed execution with configurable quotas. Not designed for AI agents specifically.
- **Key Features**: Pure Lua (no C dependencies), instruction counting for infinite loop prevention, env injection, configurable quota (default 500k instructions).
- **Status**: Active (MIT license, used in production by Kong)

### Luau VM
- **Repo**: https://github.com/luau-lang/luau
- **Stars**: ~4,500 (estimated)
- **Description**: A small, fast, and embeddable programming language derived from Lua with a gradual type system. Designed by Roblox to be safe against malicious code.
- **Agent Relevance**: Potential
- **Evidence**: Battle-tested at Roblox scale against actively malicious user code. Bytecode access removed to prevent exploits. No known sandbox escapes. Global table sandboxing between scripts.
- **Key Features**: Safe against malicious code, bytecode access removed, gradual type system, global table sandboxing, fast (JIT-like codegen).
- **Status**: Active (maintained by Roblox)

---

## Configuration Languages (Safe by Design)

### Starlark
- **Repo**: https://github.com/google/starlark-go
- **Stars**: ~2,190
- **Description**: Google's configuration language (Python dialect) — deterministic, hermetic, and safe for parallel evaluation. Used by Bazel.
- **Agent Relevance**: Potential
- **Evidence**: Designed for untrusted code execution in build systems. Deterministic evaluation, hermetic execution (no filesystem/network/clock access), parallel-safe (shared data becomes immutable). Could be used for safe agent configuration scripting.
- **Key Features**: Deterministic evaluation, hermetic execution, parallel safety, Python-like syntax, no side effects, embeddable.
- **Status**: Active (used in production by Google/Bazel)

### Jsonnet
- **Repo**: https://github.com/google/jsonnet
- **Stars**: ~7,400
- **Description**: Hermetic data templating language (JSON superset) — deterministic evaluation, no external I/O, designed for configuration.
- **Agent Relevance**: Potential
- **Evidence**: Hermetic by design — cannot perform external I/O. Google SRE recommends sandboxing untrusted Jsonnet with ulimit for additional protection. Import/importstr could be used for data exfiltration if not restricted.
- **Key Features**: Hermetic/deterministic, JSON superset, functional programming, no side effects, Go implementation for safer sandboxing.
- **Status**: Active (v0.21.0-rc1, multiple implementations)

### Dhall
- **Repo**: https://github.com/dhall-lang/dhall-lang
- **Stars**: ~4,393
- **Description**: A programmable configuration language guaranteed to terminate — not Turing-complete, formally specified, designed for safe evaluation of untrusted code.
- **Agent Relevance**: Potential
- **Evidence**: Explicitly aims to support safely importing and evaluating untrusted Dhall code, even from malicious users. Total language (always terminates), type-safe, formally specified. Only three effects possible (file import, HTTP import, env var import).
- **Key Features**: Guaranteed termination (not Turing-complete), formal specification, type-safe, only 3 possible effects, multiple implementations (Haskell, Rust, Go).
- **Status**: Active (but Rust implementation ~90% complete and unmaintained)

### CUE
- **Repo**: https://github.com/cue-lang/cue
- **Stars**: ~5,800
- **Description**: Open-source language for defining, generating, and validating configuration data — works with JSON, YAML, OpenAPI, Protobuf, and more.
- **Agent Relevance**: Potential
- **Evidence**: GitHub Issue #1373 requests "safe evaluation mode that disallows side effects" for partially-trusted users. Not yet implemented. CUE's constraint-based model is naturally restrictive but does allow module loading with potential side effects.
- **Key Features**: Constraint-based validation, data unification, schema definition, multi-format support, Go implementation.
- **Status**: Active (actively developed)

---

## TEE (Trusted Execution Environments)

### Gramine (SGX)
- **Repo**: https://github.com/gramineproject/gramine
- **Stars**: ~1,500 (estimated)
- **Description**: A library OS for running Linux multi-process applications inside Intel SGX enclaves — provides shield for applications in untrusted cloud environments.
- **Agent Relevance**: Potential
- **Evidence**: Used for confidential AI inference platforms. Enables running unmodified applications inside SGX enclaves. RA-TLS for remote attestation. Could provide hardware-level isolation for agent code execution in confidential computing scenarios.
- **Key Features**: Unmodified Linux application support, Intel SGX enclaves, RA-TLS attestation, multi-process support, filesystem encryption.
- **Status**: Active

### Occlum (SGX)
- **Repo**: https://github.com/occlum/occlum
- **Stars**: ~1,500
- **Description**: A memory-safe, multi-process library OS for Intel SGX — written in Rust, with efficient multitasking and multiple filesystem types.
- **Agent Relevance**: Potential
- **Evidence**: Memory-safe (Rust), 1000x faster enclave startup than per-enclave approaches, 3x faster IPC. Adopted by OpenHarmony and openEuler communities. Useful for confidential AI agent execution.
- **Key Features**: Memory-safe (Rust), 1000x faster startup, encrypted/hashed filesystems, partial formal verification, supports Aliyun/Azure.
- **Status**: Active (last updated Oct 2025)

### Penglai (RISC-V TEE)
- **Repo**: https://github.com/Penglai-Enclave/Penglai-Enclave
- **Stars**: ~200 (estimated)
- **Description**: Open-sourced, secure, and scalable TEE system for RISC-V — supports up to 1000 enclave instances per device.
- **Agent Relevance**: Potential
- **Evidence**: Academic research (OSDI'21). Three versions for different hardware configurations. Supports fine-grained 4KB page-level isolation. Zero-copy cross-enclave communication. Future-looking for RISC-V-based AI hardware.
- **Key Features**: Unmodified hardware support, 1000 enclave instances, zero-copy IPC, 4KB page-level isolation, partially formally verified.
- **Status**: Active (adopted by OpenHarmony and openEuler)

---

## eBPF-based Approaches

### Azazel
- **Repo**: https://github.com/beelzebub-labs/azazel
- **Stars**: ~50 (estimated, new project)
- **Description**: eBPF-powered silent observer for containerized runtimes — built for malware analysis sandboxes and Agentic AI monitoring.
- **Agent Relevance**: Direct
- **Evidence**: Explicitly designed for Agentic AI monitoring. Captures every syscall, file access, network connection, and process event in real time with zero overhead on the target container. Provides "ground-truth observability" at the kernel level, independent of what the agent logs.
- **Key Features**: Zero-overhead eBPF tracing, per-container attachment, syscall/file/network/process monitoring, real-time event capture, Docker integration.
- **Status**: Active (new/early-stage)

### eBPF for AI Agent Security (Palo Alto Networks approach)
- **Repo**: N/A (commercial/research)
- **Stars**: N/A
- **Description**: Using eBPF to observe AI agent behavior at runtime — detecting prompt injection, data exfiltration, and unauthorized system interactions in real time.
- **Agent Relevance**: Direct
- **Evidence**: Palo Alto Networks blog details using eBPF for AI security. eBPF programs observe agent-system interactions that traditional LLM firewalls (proxies) miss. Provides runtime control to detect and block dynamic threats.
- **Key Features**: Runtime behavior monitoring, prompt injection detection, data exfiltration prevention, zero kernel modification, sandboxed verification.
- **Status**: Active (research/commercial)

---

## Safe Eval Libraries

### SandboxJS
- **Repo**: https://github.com/nyariv/SandboxJS
- **Stars**: ~193
- **Description**: Safe eval runtime for JavaScript — parses and executes JS through its own runtime, checking every prototype function call for whitelisting.
- **Agent Relevance**: Potential
- **Evidence**: Provides fine-grained permission control per library (e.g., allow fetch for one, Node access for another). Multiple critical CVEs in 2026 for sandbox escapes (prototype pollution, Map.prototype.has bypass).
- **Key Features**: Prototype function whitelisting, per-library permissions, audit method for access tracking, compiled execution (can be faster than eval).
- **Status**: Active (but multiple recent security advisories)

---

## CI/CD Engines with Sandbox Properties

### Dagger
- **Repo**: https://github.com/dagger/dagger
- **Stars**: ~15,200
- **Description**: Automation engine to build, test, and ship any codebase — runs pipelines in containers with sandboxed functions and content-addressed caching.
- **Agent Relevance**: Indirect
- **Evidence**: Founded by Docker creator Solomon Hykes. Tools run in containers orchestrated by sandboxed functions. Host dependencies are explicit and strictly typed. Could be used for agent workflow orchestration with container isolation.
- **Key Features**: Container-based execution, content-addressed caching, OpenTelemetry traces, SDKs for 8 languages, Daggerverse module ecosystem.
- **Status**: Active (very actively maintained)

---

## Academic Research on Sandboxing for AI Agents

### Fault-Tolerant Sandboxing for AI Coding Agents (Dec 2025)
- **Paper**: https://arxiv.org/abs/2512.12806
- **Description**: Proposes a framework with policy-based interception and transactional filesystem snapshots — wraps agent actions in atomic transactions for safety with acceptable latency.
- **Key Contribution**: Outperforms container initialization overhead and commercial CLI friction by using atomic transactions instead of full isolation.

### Securing AI Agent Execution / AgentBound (Oct 2025)
- **Paper**: https://arxiv.org/pdf/2510.21236
- **Description**: Policy enforcement engine as a sandbox for AI agent execution — enforces declared permissions rather than detecting malicious behavior. Addresses the security gap in the MCP ecosystem.
- **Key Contribution**: Formal policy enforcement for AI agents, complementing security scanners and monitors.

### LLM-in-Sandbox Elicits General Agentic Intelligence (Jan 2026)
- **Paper**: https://arxiv.org/pdf/2601.16206
- **Description**: Demonstrates that a lightweight sandbox (standard Python + scientific computing libraries) is sufficient for general-purpose agent intelligence — delegates domain-specific tool acquisition to the model itself.
- **Key Contribution**: Shows that minimal sandboxes can be more effective than complex task-specific environments.

### Systems Security Foundations for Agentic Computing (2025)
- **Paper**: https://eprint.iacr.org/2025/2173.pdf
- **Description**: Discusses security foundations for agentic systems including sandbox-based defenses and information-flow controls as alternatives to human-in-the-loop confirmation.
- **Key Contribution**: Argues for sandboxing + information-flow controls over interactive permission prompts.

### RedCodeAgent: Automatic Red-Teaming Agent
- **Paper**: https://openreview.net/pdf?id=IyIaAOihmZ
- **Description**: Automated red-teaming to evaluate security of code agents — provides simulated sandbox environments to evaluate harmfulness of code execution results.
- **Key Contribution**: Benchmarks sandbox security across agents (OpenCodeInterpreter, ReAct, MetaGPT, Cursor, Codeium).

---

## Community Resources

### awesome-sandbox
- **Repo**: https://github.com/restyler/awesome-sandbox
- **Stars**: ~500 (estimated)
- **Description**: Curated list and analysis of modern code sandboxing solutions for AI — covers foundational technologies to full-fledged platforms.
- **Agent Relevance**: Direct
- **Evidence**: Comprehensive analysis of sandbox solutions along Security vs. Performance vs. Compatibility axes. Classifies AI agent sandboxes as "High Risk." Covers SaaS vs. self-hosted tradeoffs.
- **Key Features**: Categorized listings, risk classifications, comparison axes, regularly updated.
- **Status**: Active

---

## Summary Statistics

| Category | Count | Notable Examples |
|----------|-------|-----------------|
| JS/TS Runtimes | 4 | Deno, Bun, Val Town, Deno Sandbox |
| QuickJS Sandboxes | 2 | quickjs-emscripten, @sebastianwessel/quickjs |
| V8 Isolate | 1 | isolated-vm |
| Python Sandboxes | 8 | Pyodide, Monty, RustPython, RestrictedPython, PyPy, LangChain, Cohere, CodeJail |
| Code Execution Engines | 5 | Piston, Judge0, Compiler Explorer, Wandbox, Glot |
| Embedded Scripting | 4 | Rhai, Lune, Lua sandbox, Luau |
| Config Languages | 4 | Starlark, Jsonnet, Dhall, CUE |
| TEE/SGX | 3 | Gramine, Occlum, Penglai |
| eBPF | 2 | Azazel, Palo Alto approach |
| Safe Eval | 1 | SandboxJS |
| CI/CD | 1 | Dagger |
| Academic Papers | 5 | Various |
| **Total** | **40+** | |
