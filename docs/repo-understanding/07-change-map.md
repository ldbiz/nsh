# Change map

Use this map to locate the owner of a behaviour before modifying it.

## Shell startup or interception
- `src/shim_main.rs`, `src/shim/wrap.rs`, `src/shim/pump.rs`: wrapper and capture boundary.
- `shell/nsh.bash`, `shell/nsh.zsh`, `shell/nsh.fish`, `shell/nsh.ps1`: shell-specific hooks and prefill.
- `src/app/init.rs`: generated integration content.

**Caveat:** Shim and core have an intentional compatibility boundary. Changes to terminal capture may affect existing wrapped sessions.

## Query behaviour and context
- `src/app/query.rs`, `src/query/mod.rs`: request handling and tool loop.
- `src/query/prompt.rs`: system instructions and prompt structure.
- `src/context/mod.rs`, `src/context/project.rs`, `src/context/system_info.rs`: gathered terminal, project and environment facts.

## History, session identity and continuity
- `src/runtime/global_daemon.rs`, `src/runtime/daemon.rs`: shared runtime and IPC.
- `src/db/sessions.rs`, `src/db/conversations.rs`, `src/db/history.rs`: persisted session and conversation state.
- `src/app/session.rs`: user-facing session operations.

## Memory
- `src/memory/ingestion/`: what is retained from shell activity.
- `src/memory/retrieval/`: what is recalled for a query.
- `src/memory/store/`, `src/memory/schema.rs`: memory persistence and schema.
- `src/memory/reflection.rs`, `src/memory/decay.rs`: background lifecycle.

## Execution safety and tools
- `src/tools/command.rs`, `src/tools/run_command.rs`: command proposal/execution.
- `src/tools/runtime/`: tool dispatch and side-effect handling.
- `src/security.rs`, `src/redact.rs`: risk assessment and secret protection.
- `src/query/coding_agent.rs`: coding sub-agent.

## Provider or integration
- `src/provider/`: model adapters and routing.
- `src/mcp.rs`, `src/mcp_server.rs`: MCP client/server.
- `src/runtime/remote.rs`, `mobile/`: remote terminal access.

## Configuration and tests
- `src/config.rs`, `src/autoconfigure/`: settings and automatic discovery.
- `tests/integration.rs`, relevant module-local tests: validation.

Prefer targeted tests around the changed boundary, then `cargo test` and interactive shell acceptance for hook/shim changes.
