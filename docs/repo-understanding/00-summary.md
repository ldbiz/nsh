# nsh repository summary

## Purpose
nsh is an AI-assisted terminal environment for Bash, Zsh, Fish and PowerShell. It records shell activity, captures terminal output, assembles local and project context, and answers natural-language requests through an LLM-driven tool loop. Suggested commands can be placed into the interactive shell for review.

Unlike a simple shell alias, nsh includes a PTY-wrapping shim, a shared daemon, persistent SQLite history, memory, provider integrations and tools capable of performing work.

## Stack and runtime
- Rust 2024 workspace: main crate and `proto/` IPC crate; Tokio async runtime.
- SQLite through rusqlite, HTTP streaming through reqwest, shell-specific integration scripts.
- Separate `nsh` shim and `nsh-core` executable; optional iroh-based remote access (enabled by default).
- Optional mobile companion under `mobile/` (Tauri 2, TypeScript and xterm.js).

The stable shim handles terminal wrapping and capture, while the core handles CLI operations, the global daemon, query orchestration, providers, memory and tools. Shell hooks report command completion and coordinate command prefill. The daemon maintains shared state and database access across terminal sessions.

## External integrations
Configured LLM providers, optional web search and MCP integrations, OS commands and filesystem access, Git metadata, and optional iroh remote networking.

## Main entry points
- `src/shim_main.rs`: `nsh` binary, wrapper boundary and core dispatch.
- `src/core_main.rs`, `src/app.rs`, `src/cli.rs`: core CLI dispatch.
- `shell/nsh.{bash,zsh,fish,ps1}`: interactive shell hooks.
- `src/runtime/global_daemon.rs`: daemon lifecycle.
- `src/query/mod.rs`: AI request and tool loop.

## Read these first
1. `README.md` — intended user experience and supported features.
2. `Cargo.toml` — binaries, workspace and dependencies.
3. `src/shim_main.rs` and `src/shim/` — terminal interception boundary.
4. `shell/nsh.bash` — Bash hook behaviour.
5. `src/app.rs` and `src/cli.rs` — CLI routing.
6. `src/runtime/global_daemon.rs` — shared runtime.
7. `src/query/mod.rs` — request lifecycle.
8. `src/context/` — environment/project context.
9. `src/memory/` and `src/db/` — persistent context.
10. `src/security.rs` and `src/tools/` — command safety and actions.

See the remaining files in this pack for runtime paths, change locations and limitations.
