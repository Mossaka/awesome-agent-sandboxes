# Getting Started with Agent Sandboxes

> Quick installation and setup guides for the most popular agent sandboxing solutions, verified from official documentation.

## Table of Contents

1. [E2B](#1-e2b) — Cloud sandbox for AI agents (Firecracker microVMs)
2. [Daytona](#2-daytona) — Secure infrastructure for AI-generated code
3. [Docker Sandboxes](#3-docker-sandboxes) — Official Docker agent sandbox
4. [Firecracker](#4-firecracker) — Secure microVMs for serverless computing
5. [gVisor](#5-gvisor) — Application kernel for container sandboxing
6. [microsandbox](#6-microsandbox) — Self-hosted agent sandbox (libkrun microVMs)
7. [nsjail](#7-nsjail) — Lightweight process isolation
8. [Bubblewrap](#8-bubblewrap) — Unprivileged sandboxing (used by Claude Code)
9. [Wasmtime](#9-wasmtime) — WebAssembly runtime with sandbox isolation
10. [Deno](#10-deno) — Permission-based JavaScript/TypeScript runtime
11. [Piston](#11-piston) — Multi-language code execution engine
12. [OpenHands](#12-openhands) — AI coding agent with Docker sandbox
13. [nono](#13-nono) — Kernel-enforced AI agent sandbox
14. [Kata Containers](#14-kata-containers) — Lightweight VMs for container isolation
15. [Judge0](#15-judge0) — Online code execution system

---

## 1. E2B

Cloud-hosted secure sandboxes for AI agents, powered by Firecracker microVMs with sub-200ms startup.

### Prerequisites

- Node.js 18+ or Python 3.8+
- An E2B account and API key from [e2b.dev/dashboard](https://e2b.dev/dashboard?tab=keys)

### Installation

**Python:**

```bash
pip install e2b-code-interpreter
```

**JavaScript/TypeScript:**

```bash
npm i @e2b/code-interpreter
```

### Quick Start

Set your API key:

```bash
export E2B_API_KEY=e2b_***
```

**Python:**

```python
from e2b_code_interpreter import Sandbox

with Sandbox.create() as sandbox:
    sandbox.run_code("x = 1")
    execution = sandbox.run_code("x+=1; x")
    print(execution.text)  # outputs 2
```

**JavaScript/TypeScript:**

```typescript
import { Sandbox } from '@e2b/code-interpreter'

const sandbox = await Sandbox.create()
await sandbox.runCode('x = 1')
const execution = await sandbox.runCode('x+=1; x')
console.log(execution.text)  // outputs 2
```

### Agent Integration Notes

E2B is purpose-built for AI agents. SDKs integrate directly with LangChain, CrewAI, OpenAI Assistants, and Anthropic tool use. Used by Manus AI, Groq, HuggingFace, and Perplexity.

### Quality Assessment

| Docs | Active | CI | Tests | Stars |
|------|--------|----|-------|-------|
| ✅ Excellent | ✅ v2.14.0 (Feb 2026) | ✅ | ✅ | ~11,000 |

**Gotchas:** Cloud-hosted only (no self-hosting). Requires internet access and API key. Free tier has usage limits.

---

## 2. Daytona

Secure and elastic infrastructure for running AI-generated code with sub-90ms sandbox creation.

### Prerequisites

- Python 3.8+, Node.js 18+, Go 1.21+, or Ruby
- A Daytona account at [app.daytona.io](https://app.daytona.io)
- API key from the Daytona dashboard

### Installation

**Python:**

```bash
pip install daytona
```

**TypeScript:**

```bash
npm install @daytonaio/sdk
```

### Quick Start

**Python:**

```python
from daytona import Daytona, DaytonaConfig, CreateSandboxBaseParams

daytona = Daytona(DaytonaConfig(api_key="YOUR_API_KEY"))

sandbox = daytona.create(CreateSandboxBaseParams(language="python"))

response = sandbox.process.code_run('print("Sum: " + str(3 + 4))')

if response.exit_code != 0:
    print(f"Error: {response.exit_code} {response.result}")
else:
    print(response.result)

daytona.delete(sandbox)
```

### Agent Integration Notes

Daytona provides native integrations with LangChain, Turing, Writer, and SambaNova. SDKs support Python, TypeScript, Go, and Ruby. OCI/Docker compatible images.

### Quality Assessment

| Docs | Active | CI | Tests | Stars |
|------|--------|----|-------|-------|
| ✅ Excellent | ✅ v0.146.0 (Feb 2026) | ✅ | ✅ | ~60,400 |

**Gotchas:** Cloud-hosted SaaS. $24M Series A (Feb 2026) — well-funded but relatively new pivot to AI agent infra.

---

## 3. Docker Sandboxes

Docker's official microVM-based sandbox for AI coding agents, released December 2025.

### Prerequisites

- **Docker Desktop 4.40+** (macOS or Windows)
- Linux: Docker Desktop 4.57+ (container-based fallback, not microVM)

### Installation

Docker Sandboxes are built into Docker Desktop — no separate installation needed. Update Docker Desktop to the latest version.

### Quick Start

Navigate to your project directory and run:

```bash
docker sandbox run claude
```

Replace `claude` with your preferred agent (`gemini`, `codex`, `copilot`, `kiro`).

This creates an isolated microVM sandbox with:
- A private Docker daemon per sandbox
- Automatic file synchronization at matching absolute paths
- Full network isolation from the host

### Agent Integration Notes

Supports Claude Code, Gemini CLI, OpenAI Codex, GitHub Copilot, and Kiro out of the box. Each agent runs in a fully isolated environment with its own Docker daemon.

### Quality Assessment

| Docs | Active | CI | Tests | Stars |
|------|--------|----|-------|-------|
| ✅ Good | ✅ Rapidly evolving | ✅ | ✅ | N/A (closed-source) |

**Gotchas:** MicroVM isolation requires macOS or Windows. Linux falls back to container-based isolation. Part of Docker Desktop (not open-source). Experimental feature — rapidly changing.

---

## 4. Firecracker

Secure and fast microVMs for serverless computing by AWS. <5 MiB memory overhead, ~125ms boot time.

### Prerequisites

- **Linux host with KVM support** (`/dev/kvm` must exist)
- An uncompressed Linux kernel binary
- An ext4 rootfs image

### Installation

Download the latest release binary:

```bash
ARCH="$(uname -m)"
release_url="https://github.com/firecracker-microvm/firecracker/releases"
latest=$(basename $(curl -fsSLI -o /dev/null -w %{url_effective} ${release_url}/latest))
curl -L ${release_url}/download/${latest}/firecracker-${latest}-${ARCH}.tgz | tar -xz
mv release-${latest}-$(uname -m)/firecracker-${latest}-${ARCH} firecracker
```

### Quick Start

Running Firecracker requires **two terminals**:

**Terminal 1 — Start Firecracker:**

```bash
API_SOCKET="/tmp/firecracker.socket"
sudo rm -f $API_SOCKET
sudo ./firecracker --api-sock "${API_SOCKET}"
```

**Terminal 2 — Configure and boot the microVM via HTTP API:**

```bash
# Set the kernel boot source
curl --unix-socket "${API_SOCKET}" -X PUT "http://localhost/boot-source" \
  -H "Content-Type: application/json" \
  -d '{"kernel_image_path": "./vmlinux", "boot_args": "console=ttyS0 reboot=k panic=1 pci=off"}'

# Attach root filesystem
curl --unix-socket "${API_SOCKET}" -X PUT "http://localhost/drives/rootfs" \
  -H "Content-Type: application/json" \
  -d '{"drive_id": "rootfs", "path_on_host": "./rootfs.ext4", "is_root_device": true, "is_read_only": false}'

# Start the microVM
curl --unix-socket "${API_SOCKET}" -X PUT "http://localhost/actions" \
  -H "Content-Type: application/json" \
  -d '{"action_type": "InstanceStart"}'
```

### Agent Integration Notes

Firecracker is the foundation for E2B, Vercel Sandbox, and AWS Lambda/Fargate. It's not an agent tool directly — it's the underlying VM technology that agent platforms build on. Use E2B or microsandbox for a higher-level agent SDK.

### Quality Assessment

| Docs | Active | CI | Tests | Stars |
|------|--------|----|-------|-------|
| ✅ Excellent | ✅ v1.14.1 (Jan 2026) | ✅ | ✅ | ~32,700 |

**Gotchas:** Linux-only, requires KVM (bare metal or nested virtualization). Requires root/sudo. Low-level — you must provide your own kernel and rootfs images. In production, use the `jailer` binary for additional isolation.

---

## 5. gVisor

Application kernel that implements the Linux interface in user-space Go. Drop-in replacement for runc via `runsc`.

### Prerequisites

- **Linux 4.14.77+**
- Docker 17.09.0+ or containerd

### Installation

**Via apt (Debian/Ubuntu):**

```bash
sudo apt-get update && sudo apt-get install -y \
    apt-transport-https ca-certificates curl gnupg

curl -fsSL https://gvisor.dev/archive.key | sudo gpg --dearmor -o /usr/share/keyrings/gvisor-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/gvisor-archive-keyring.gpg] https://storage.googleapis.com/gvisor/releases release main" | sudo tee /etc/apt/sources.list.d/gvisor.list > /dev/null

sudo apt-get update && sudo apt-get install -y runsc
```

**Manual download:**

```bash
ARCH=$(uname -m)
URL=https://storage.googleapis.com/gvisor/releases/release/latest/${ARCH}
wget ${URL}/runsc ${URL}/runsc.sha512
sha512sum -c runsc.sha512
rm -f runsc.sha512
chmod a+rx runsc
sudo mv runsc /usr/local/bin
```

### Quick Start

Configure Docker and run a sandboxed container:

```bash
# Install gVisor as a Docker runtime
sudo /usr/local/bin/runsc install
sudo systemctl reload docker

# Run a container with gVisor isolation
docker run --rm --runtime=runsc hello-world

# Run an interactive sandboxed shell
docker run --rm -it --runtime=runsc ubuntu /bin/bash
```

### Agent Integration Notes

Core technology for GKE Sandbox, kubernetes-sigs/agent-sandbox, Modal, and Northflank. Supports GPU/CUDA passthrough. Used by Google internally for Search, Gmail, and Cloud Functions.

### Quality Assessment

| Docs | Active | CI | Tests | Stars |
|------|--------|----|-------|-------|
| ✅ Excellent (gvisor.dev) | ✅ 10,712 commits | ✅ | ✅ | ~17,800 |

**Gotchas:** Linux-only. Not all syscalls are implemented — some workloads may not be compatible. Not a VM — it's an application kernel (different threat model from hardware virtualization).

---

## 6. microsandbox

Open-source self-hosted sandboxes for AI agents using libkrun microVMs with sub-200ms boot and MCP integration.

### Prerequisites

- Linux or macOS (Windows WIP)
- KVM support (Linux) or HVF (macOS)

### Installation

```bash
curl -sSL https://get.microsandbox.dev | sh
```

### Quick Start

**Start the server:**

```bash
msb server start --dev
```

**Pull an environment image (optional):**

```bash
msb pull microsandbox/python
```

**Run a temporary sandbox:**

```bash
msx python
```

**Create a persistent system-wide sandbox:**

```bash
msi python py-data    # Creates alias 'py-data'
py-data               # Run from any directory
```

### Agent Integration Notes

Built-in MCP server for direct agent integration. Documented at [docs.microsandbox.dev](https://docs.microsandbox.dev). Listed on the Glama MCP platform. Self-hosted alternative to E2B.

### Quality Assessment

| Docs | Active | CI | Tests | Stars |
|------|--------|----|-------|-------|
| ✅ Good (docs.microsandbox.dev) | ✅ Experimental | ✅ | ✅ | ~4,800 |

**Gotchas:** Experimental status — API may change. Requires hardware virtualization support. Newer project with smaller community than E2B.

---

## 7. nsjail

Lightweight process isolation tool by Google using Linux namespaces, cgroups, rlimits, and seccomp-bpf with Kafel DSL.

### Prerequisites

- **Linux** (uses kernel namespaces)
- Build tools: `autoconf bison flex gcc g++ git libprotobuf-dev libnl-route-3-dev libtool make pkg-config protobuf-compiler`

### Installation

**Build from source:**

```bash
sudo apt-get install -y autoconf bison flex gcc g++ git libprotobuf-dev \
    libnl-route-3-dev libtool make pkg-config protobuf-compiler

git clone https://github.com/google/nsjail.git
cd nsjail
make
```

**Via Docker:**

```bash
docker build -t nsjail .
docker run --privileged --rm -it nsjail nsjail --user 99999 --group 99999 --chroot / -- /bin/bash
```

### Quick Start

**Run an isolated shell:**

```bash
./nsjail -Mo --chroot / --user 99999 --group 99999 -- /bin/bash
```

**Run a network service (listen mode):**

```bash
./nsjail -Ml --port 9000 --chroot /chroot --user 99999 --group 99999 -- /bin/sh -i
```

**Execute a command in isolation:**

```bash
./nsjail -Mr --chroot / -- /bin/echo "Hello from sandbox"
```

Modes: `-Mo` (once), `-Ml` (listen/server), `-Mr` (repeated execution).

### Agent Integration Notes

Used by Windmill for sandboxing code execution. Well-suited for hosting code execution services (CTFs, online judges). Use listen mode (`-Ml`) to create a sandboxed execution service.

### Quality Assessment

| Docs | Active | CI | Tests | Stars |
|------|--------|----|-------|-------|
| ✅ Good (README + man page) | ✅ nsjail-3.4 | ✅ | ✅ | ~3,700 |

**Gotchas:** Linux-only. Requires root or appropriate capabilities for namespace creation. Build from source (no pre-built packages for most distros). Configuration via protobuf config files for complex setups.

---

## 8. Bubblewrap

Low-level unprivileged sandboxing tool using Linux namespaces. Used by Flatpak and Anthropic's Claude Code.

### Prerequisites

- **Linux** with user namespace support (most modern kernels)
- For building from source: Meson build system

### Installation

**From package manager (recommended):**

```bash
# Debian/Ubuntu
sudo apt-get install bubblewrap

# Fedora
sudo dnf install bubblewrap

# Arch Linux
sudo pacman -S bubblewrap
```

**From source:**

```bash
git clone https://github.com/containers/bubblewrap.git
cd bubblewrap
meson _builddir
meson compile -C _builddir
sudo meson install -C _builddir
```

### Quick Start

**Run a sandboxed shell with minimal filesystem access:**

```bash
bwrap \
    --ro-bind /usr /usr \
    --symlink usr/lib64 /lib64 \
    --proc /proc \
    --dev /dev \
    --unshare-pid \
    --new-session \
    bash
```

**Run a command with restricted filesystem and no network:**

```bash
bwrap \
    --ro-bind /usr /usr \
    --ro-bind /etc /etc \
    --symlink usr/lib /lib \
    --symlink usr/lib64 /lib64 \
    --symlink usr/bin /bin \
    --symlink usr/sbin /sbin \
    --proc /proc \
    --dev /dev \
    --tmpfs /tmp \
    --unshare-all \
    --new-session \
    -- python3 -c "print('Hello from sandbox')"
```

### Agent Integration Notes

Used by Anthropic's Claude Code sandbox runtime on Linux (`@anthropic-ai/sandbox-runtime` uses bwrap). Significantly faster than Docker for lightweight sandboxing. Ideal for wrapping individual command executions from AI agents.

### Quality Assessment

| Docs | Active | CI | Tests | Stars |
|------|--------|----|-------|-------|
| ✅ Good | ✅ v0.11.0 (Oct 2024) | ✅ | ✅ | ~5,900 |

**Gotchas:** Linux-only. Low-level tool — requires understanding of namespaces and bind mounts. No built-in networking isolation beyond `--unshare-net`. No root required (uses unprivileged user namespaces).

---

## 9. Wasmtime

Standalone WebAssembly runtime by the Bytecode Alliance. Sandboxed by default — Wasm modules cannot access the host unless explicitly granted.

### Prerequisites

- For running: no special requirements
- For compiling Wasm from Rust: `rustup` with `wasm32-wasip2` target

### Installation

**Linux/macOS:**

```bash
curl https://wasmtime.dev/install.sh -sSf | bash
```

**Windows:** Download from the [GitHub Releases page](https://github.com/bytecodealliance/wasmtime/releases).

### Quick Start

**Compile and run a Rust program as Wasm:**

```bash
# Add the Wasm target
rustup target add wasm32-wasip2

# Create and compile a hello-world program
echo 'fn main() { println!("Hello from Wasm sandbox!"); }' > hello.rs
rustc hello.rs --target wasm32-wasip2

# Run in sandboxed Wasmtime
wasmtime hello.wasm
```

**Run with explicit directory access (capability-based security):**

```bash
wasmtime --dir ./data hello.wasm
```

Without `--dir`, the Wasm module has **zero** filesystem access.

### Agent Integration Notes

Foundation for Wasm-based agent sandboxing (Wassette by Microsoft, Extism, Spin by Fermyon). Capability-based security model: modules start with no access and must be explicitly granted permissions. Supports Python/JS via compilation to Wasm (e.g., via Pyodide, componentize-py).

### Quality Assessment

| Docs | Active | CI | Tests | Stars |
|------|--------|----|-------|-------|
| ✅ Excellent | ✅ Frequent releases | ✅ | ✅ | ~15,700+ |

**Gotchas:** Not all languages compile to Wasm easily. WASI is still maturing — some POSIX features may be missing. Best for sandboxing individual modules/functions rather than full OS environments.

---

## 10. Deno

Secure JavaScript/TypeScript runtime with permission-based sandboxing. Programs have no access to the filesystem, network, or environment unless explicitly granted.

### Prerequisites

- No special requirements

### Installation

**macOS/Linux:**

```bash
curl -fsSL https://deno.land/install.sh | sh
```

**Homebrew (macOS):**

```bash
brew install deno
```

**Windows (PowerShell):**

```powershell
irm https://deno.land/install.ps1 | iex
```

### Quick Start

**Run a script with no permissions (fully sandboxed):**

```bash
echo 'console.log("Hello from sandbox!")' > hello.ts
deno run hello.ts
```

**Grant specific permissions:**

```bash
# Allow network access only
deno run --allow-net server.ts

# Allow reading specific directory only
deno run --allow-read=./data script.ts

# Allow everything (not recommended for untrusted code)
deno run --allow-all script.ts
```

**Permission flags:**
- `--allow-read[=<path>]` — Filesystem read
- `--allow-write[=<path>]` — Filesystem write
- `--allow-net[=<host>]` — Network access
- `--allow-env[=<var>]` — Environment variables
- `--allow-run[=<prog>]` — Subprocess execution
- `--deny-*` — Explicitly deny specific permissions

### Agent Integration Notes

Used by smolagents (HuggingFace) as a sandbox backend (Pyodide+Deno). Permission model maps naturally to agent capability restrictions. Deno Sandbox (cloud product) provides hosted sandboxed execution for agents.

### Quality Assessment

| Docs | Active | CI | Tests | Stars |
|------|--------|----|-------|-------|
| ✅ Excellent (docs.deno.com) | ✅ Frequent releases | ✅ | ✅ | ~101,000+ |

**Gotchas:** JavaScript/TypeScript only (or Wasm). Permission model is per-process — not as granular as per-function sandboxing. Some npm packages may need `--allow-all` to work.

---

## 11. Piston

Multi-language code execution engine supporting 100+ languages. Sandboxed execution via Linux namespaces and cgroups.

### Prerequisites

- Docker and Docker Compose
- Node.js 15+ (for CLI management)
- Linux with cgroup v2 enabled, cgroup v1 disabled

### Installation

**Quick Docker setup:**

```bash
docker run --privileged -v $PWD:/piston -dit -p 2000:2000 \
    --name piston_api ghcr.io/engineer-man/piston
```

**Full setup with CLI:**

```bash
git clone https://github.com/engineer-man/piston
cd piston
docker-compose up -d api
cd cli && npm i && cd -
```

### Quick Start

**Install a language runtime:**

```bash
cli/index.js ppman install python
```

**Execute code via CLI:**

```bash
echo 'print("Hello from Piston!")' > test.py
cli/index.js run python test.py
```

**Execute code via API:**

```bash
curl -X POST http://localhost:2000/api/v2/execute \
    -H "Content-Type: application/json" \
    -d '{
        "language": "python",
        "version": "3.10.0",
        "files": [{"content": "print(\"Hello from Piston!\")"}]
    }'
```

### Agent Integration Notes

REST API makes it easy to integrate with any agent framework. Supports 100+ languages including Python, JavaScript, Go, Rust, C++. Each execution runs in an isolated namespace with resource limits.

### Quality Assessment

| Docs | Active | CI | Tests | Stars |
|------|--------|----|-------|-------|
| ✅ Good | ✅ Active | ✅ | ✅ | ~5,200+ |

**Gotchas:** Requires `--privileged` Docker flag. Linux-only for self-hosting. API starts with no language runtimes — must install them separately. cgroup v2 required (cgroup v1 must be disabled).

---

## 12. OpenHands

AI-powered autonomous development platform (formerly OpenDevin) with built-in Docker sandbox isolation.

### Prerequisites

- Python 3.12+ (for CLI)
- Docker (for sandbox execution)
- An LLM API key (OpenAI, Anthropic, etc.)

### Installation

**Via uv (recommended):**

```bash
uv tool install openhands --python 3.12
```

**Via install script:**

```bash
curl -fsSL https://install.openhands.dev/install.sh | sh
```

**Via Docker:**

```bash
docker run -it --pull=always \
    -e SANDBOX_USER_ID=$(id -u) \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v ~/.openhands:/root/.openhands \
    --add-host host.docker.internal:host-gateway \
    --name openhands-app \
    ghcr.io/all-hands-ai/openhands:latest
```

### Quick Start

**CLI mode:**

```bash
# Launch interactive CLI
openhands

# Start with a specific task
openhands -t "Create a REST API for user management"

# Use a task file
openhands -f requirements.txt
```

**Key controls:** `Ctrl+P` (command palette), `Esc` (pause agent), `Ctrl+Q` (exit).

### Agent Integration Notes

OpenHands IS the agent — it includes the sandbox as part of its architecture. Every code execution happens inside a Docker container. Also provides a Software Agent SDK for building custom agents.

### Quality Assessment

| Docs | Active | CI | Tests | Stars |
|------|--------|----|-------|-------|
| ✅ Excellent (docs.openhands.dev) | ✅ v1.4.0 (Feb 2026) | ✅ | ✅ | ~68,200 |

**Gotchas:** Requires Docker socket access. Windows requires WSL. Substantial resource usage (runs a full agent + sandbox). Not a standalone sandbox — it's a complete agent platform.

---

## 13. nono

Kernel-enforced sandbox CLI/SDKs for AI agents with capability-based isolation, key management, atomic rollback, and audit trails. Created by Luke Hinds (Sigstore creator).

### Prerequisites

- macOS or Linux
- Homebrew (macOS, for installation)

### Installation

**macOS (Homebrew):**

```bash
brew tap always-further/nono
brew install nono
```

**Linux:** Download prebuilt binaries from [nono.sh](https://nono.sh) or see [docs.nono.sh/installation](https://docs.nono.sh/installation).

### Quick Start

**Sandbox a command with filesystem restrictions:**

```bash
nono run --read ./src --write ./output -- cargo build
```

This grants read access to `./src` and write access to `./output`. The kernel blocks all other filesystem operations.

**Use a pre-built agent profile:**

```bash
# Run Claude Code in a nono sandbox
nono run --profile claude-code -- claude

# Run OpenAI Codex in a nono sandbox
nono run --profile codex -- codex
```

### Agent Integration Notes

Ships with pre-built profiles for Claude Code, Codex, and OpenCode. Uses Landlock (Linux) and Seatbelt (macOS) for kernel-level enforcement. Restrictions are irreversible once applied — inherited by all child processes.

### Quality Assessment

| Docs | Active | CI | Tests | Stars |
|------|--------|----|-------|-------|
| ✅ Good (docs.nono.sh) | ✅ Early stage | ✅ | ✅ | ~586 |

**Gotchas:** Early-stage project. Landlock requires Linux kernel 5.13+ (ABI v1) or 6.7+ (ABI v5 for full features). macOS support via Seatbelt (deprecated but functional). Smaller community.

---

## 14. Kata Containers

Lightweight VMs that feel and perform like containers, with hardware virtualization isolation. Supports QEMU, Cloud Hypervisor, and Firecracker as hypervisors.

### Prerequisites

- **Linux** (x86_64, aarch64, ppc64le, or s390x)
- KVM support (bare metal or nested virtualization)
- containerd or Docker

### Installation

**Via kata-manager script (recommended for Ubuntu):**

```bash
wget https://raw.githubusercontent.com/kata-containers/kata-containers/main/utils/kata-manager.sh
chmod +x kata-manager.sh

# Install with containerd (default)
./kata-manager.sh

# Or install with Docker
./kata-manager.sh -D

# Install Kata only (no container runtime)
./kata-manager.sh -o
```

**Verify installation:**

```bash
./kata-manager.sh -l
kata-runtime check
```

### Quick Start

**Run a container with Kata isolation (Docker):**

```bash
docker run --rm --runtime=kata-runtime hello-world
```

**Verify VM-based isolation** (kernel version will differ from host):

```bash
docker run --rm --runtime=kata-runtime ubuntu uname -a
```

### Agent Integration Notes

Integrated with kubernetes-sigs/agent-sandbox (announced at KubeCon NA 2025). Used by Northflank (2M+ microVMs/month). Supported on Azure AKS. Provides stronger isolation than gVisor (full VM boundary) at the cost of slightly higher overhead.

### Quality Assessment

| Docs | Active | CI | Tests | Stars |
|------|--------|----|-------|-------|
| ✅ Good (katacontainers.io) | ✅ v3.27.0 (Feb 2026) | ✅ | ✅ | ~7,500 |

**Gotchas:** Requires hardware virtualization (KVM). Higher startup latency than gVisor (~100ms vs ~10ms). Not all Docker features are compatible. Kata Containers 3.x (Rust runtime) is the current version — avoid older 1.x/2.x guides.

---

## 15. Judge0

Robust, scalable, and sandboxed online code execution system supporting 60+ languages. Used for competitive programming platforms, e-learning, and candidate assessment.

### Prerequisites

- Docker and Docker Compose
- Linux or macOS (Windows: use WSL)

### Installation

**Download and deploy:**

```bash
# Download the latest release
wget https://github.com/judge0/judge0/releases/download/v1.13.1/judge0-v1.13.1.zip
unzip judge0-v1.13.1.zip
cd judge0-v1.13.1

# Edit configuration (optional)
# vi judge0.conf

# Start all services (API + worker + PostgreSQL + Redis)
docker-compose up -d
```

The API will be available at `http://localhost:2358`.

### Quick Start

**Execute code via API:**

```bash
curl -X POST http://localhost:2358/submissions?wait=true \
    -H "Content-Type: application/json" \
    -d '{
        "language_id": 109,
        "source_code": "print(f\"hello, {input()}\")",
        "stdin": "Alice"
    }'
```

**Python SDK:**

```bash
pip install judge0
```

```python
import judge0

result = judge0.run(
    source_code='print(f"hello, {input()}")',
    stdin="Alice",
    language=judge0.PYTHON
)
print(result.stdout)  # hello, Alice
```

### Agent Integration Notes

REST API integrates easily with any agent framework. Well-suited for building code execution tools for LLM agents. Supports batched submissions for parallel execution. The Python SDK can connect to self-hosted or cloud instances.

### Quality Assessment

| Docs | Active | CI | Tests | Stars |
|------|--------|----|-------|-------|
| ✅ Good (ce.judge0.com) | ✅ v1.13.1 | ✅ | ✅ | ~2,800+ |

**Gotchas:** Requires `--privileged` Docker containers. Four services required (API, worker, PostgreSQL, Redis). Linux/macOS only for self-hosting. Resource-intensive for self-hosted deployments.

---

## Comparison Matrix

| Project | Type | Self-Hosted | Cloud | Linux | macOS | Windows | Startup | Agent SDK |
|---------|------|-------------|-------|-------|-------|---------|---------|-----------|
| E2B | MicroVM (Firecracker) | ❌ | ✅ | ✅ | ✅ | ✅ | ~200ms | ✅ Python/TS |
| Daytona | Container | ❌ | ✅ | ✅ | ✅ | ✅ | ~90ms | ✅ Python/TS/Go/Ruby |
| Docker Sandboxes | MicroVM | ❌ | ✅ (Desktop) | ⚠️ Fallback | ✅ | ✅ | Fast | CLI only |
| Firecracker | MicroVM | ✅ | ❌ | ✅ | ❌ | ❌ | ~125ms | HTTP API |
| gVisor | App Kernel | ✅ | ❌ | ✅ | ❌ | ❌ | ~10ms | Docker/K8s |
| microsandbox | MicroVM (libkrun) | ✅ | ❌ | ✅ | ✅ | 🚧 | ~200ms | ✅ MCP |
| nsjail | Namespace | ✅ | ❌ | ✅ | ❌ | ❌ | ~1ms | CLI/protobuf |
| Bubblewrap | Namespace | ✅ | ❌ | ✅ | ❌ | ❌ | ~1ms | CLI |
| Wasmtime | Wasm | ✅ | ❌ | ✅ | ✅ | ✅ | ~1ms | Embedding API |
| Deno | Permission | ✅ | ✅ | ✅ | ✅ | ✅ | ~10ms | CLI/API |
| Piston | Namespace | ✅ | ❌ | ✅ | ❌ | ❌ | ~50ms | REST API |
| OpenHands | Container | ✅ | ✅ | ✅ | ✅ | ⚠️ WSL | ~5s | ✅ Python SDK |
| nono | Kernel (Landlock/Seatbelt) | ✅ | ❌ | ✅ | ✅ | ❌ | ~1ms | CLI/profiles |
| Kata Containers | Lightweight VM | ✅ | ❌ | ✅ | ❌ | ❌ | ~100ms | Docker/K8s |
| Judge0 | Container | ✅ | ✅ | ✅ | ✅ | ⚠️ WSL | ~100ms | REST/Python SDK |

---

## Choosing the Right Sandbox

**For the fastest start with AI agents:**
- **E2B** or **Daytona** — Cloud SDKs, no infrastructure to manage

**For self-hosted agent sandboxing:**
- **microsandbox** — MicroVM isolation with MCP integration
- **Piston** or **Judge0** — Multi-language code execution services

**For wrapping CLI agents (Claude Code, Codex):**
- **nono** — Pre-built profiles for popular agents
- **Bubblewrap** — Used by Claude Code itself
- **Docker Sandboxes** — Official Docker solution

**For Kubernetes deployments:**
- **gVisor** + kubernetes-sigs/agent-sandbox
- **Kata Containers** for stronger VM-level isolation

**For embedding in applications:**
- **Wasmtime** — Capability-based Wasm sandbox
- **Deno** — Permission-based JS/TS runtime

**For maximum isolation (defense in depth):**
- **Firecracker** (microVM) + **gVisor** (app kernel) + **nsjail** (process isolation)
