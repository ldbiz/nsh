# Important files and directories

Only the principal architectural entry points and boundaries are listed.

## Entrypoints and shell boundary
| File / directory | Role | Why it matters |
|---|---|---|
| `src/shim_main.rs` | Stable executable entry | Chooses wrapping versus delegation to the core |
| `src/shim/` | PTY wrapper, capture and pumping | Explains how nsh observes terminal output without replacing the shell |
| `src/core_main.rs`, `src/app.rs`, `src/cli.rs` | Core command dispatch | Defines available operations and where they lead |
| `shell/nsh.bash`, `shell/nsh.zsh`, `shell/nsh.fish`, `shell/nsh.ps1` | Shell integration | Records commands and manages shell-specific interaction |

## Core runtime
| File / directory | Role | Why it matters |
|---|---|---|
| `src/runtime/global_daemon.rs` | Global daemon lifecycle | Owns shared session state and background work |
| `src/runtime/daemon.rs`, `src/runtime/daemon_client.rs` | IPC protocol and client | Connects shell/core requests to the daemon |
| `proto/src/lib.rs` | Shim/core wire protocol | Important compatibility boundary |
| `src/query/mod.rs` | Query and tool loop | Main AI orchestration path |
| `src/query/prompt.rs` | System prompt construction | Combines instructions with gathered context |
| `src/tools/` | Tool implementations and runtime | Defines agent capabilities and side effects |
| `src/provider/` | LLM adapters and routing | Determines backend-specific API behaviour |

## Context, persistence and safeguards
| File / directory | Role | Why it matters |
|---|---|---|
| `src/context/` | Query context assembly | OS, terminal, project, Git and history information |
| `src/db/` | SQLite persistence | Commands, sessions, conversations, memory-related records |
| `src/memory/` | Long-term memory lifecycle | Ingestion, retrieval, decay, reflection and storage |
| `src/config.rs` | Typed configuration and defaults | Central settings contract |
| `src/security.rs`, `src/redact.rs` | Risk assessment and secret handling | Key safety boundaries |
| `src/mcp.rs`, `src/mcp_server.rs` | MCP integrations | External tools and exposing a limited tool subset |

## Tests and distribution
| File / directory | Role | Why it matters |
|---|---|---|
| `tests/integration.rs` | CLI integration tests | Tests built executable with isolated home |
| `Cargo.toml` | Workspace, binaries, feature flags | Reproducible build and test entry |
| `install.sh`, `install.ps1` | Installation | Shell setup and managed binaries |
