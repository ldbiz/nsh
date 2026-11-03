# Behaviour walkthroughs

## 1. Capture a normal shell command
**Trigger:** A user runs a regular shell command.

**Modules:** `shell/nsh.bash` (or another shell hook), `src/shim/pump.rs`, `src/runtime/daemon.rs`, `src/runtime/global_daemon.rs`, `src/db/`.

**Flow:** Shell hooks identify command completion, exit status, working directory and session. PTY capture retains recent terminal output. Daemon operations store the event and make it available to later context/history queries.

**Inputs:** Command, session, CWD, status, timing and captured output. **Outputs:** Persisted history and session state. **Side effects:** SQLite writes and daemon communication.

**Tests:** `tests/integration.rs` includes command-recording tests; `src/db/tests.rs` covers persistence.

## 2. Answer a natural-language request
**Trigger:** The `?` shell alias or `nsh query`.

**Modules:** `src/app/query.rs`, `src/query/mod.rs`, `src/query/prompt.rs`, `src/context/`, `src/memory/retrieval/`, `src/provider/`, `src/tools/`.

**Flow:** The query handler resolves the current session and delegates to the query engine. Context and relevant memories inform a constructed prompt. A provider response may invoke several tools; results feed back into the ongoing exchange.

**Inputs:** User request, session state, configuration and local context. **Outputs:** Text, tool results or suggested commands. **Side effects:** Provider API calls, history writes and tool-specific actions.

**Tests:** Query-loop tests in `src/query/mod.rs`, prompt tests in `src/query/prompt.rs`.

## 3. Suggest and execute a command
**Trigger:** The model selects a command-related tool.

**Modules:** `src/tools/command.rs`, `src/tools/run_command.rs`, `src/security.rs`, `src/config.rs`, shell integration scripts.

**Flow:** A suggested command is assessed against execution policy. The normal path places it into the user's shell buffer for inspection; other configured modes may confirm or execute. Intermediate agent commands can follow different policy from final suggestions.

**Inputs:** Proposed command, execution mode and risk classification. **Outputs:** Prefill/approval prompt or execution result. **Side effects:** Potential shell execution and filesystem changes.

**Tests:** Command-risk tests in `src/security.rs`, tool tests in `src/tools/tests.rs`.

## 4. Maintain and retrieve memory
**Trigger:** Shell events, query-time retrieval or explicit `nsh memory` operations.

**Modules:** `src/memory/ingestion/`, `src/memory/retrieval/`, `src/memory/store/`, `src/memory/reflection.rs`, `src/db/`.

**Flow:** Ingestion filters and routes useful activity into memory stores. Retrieval ranks context relevant to a new request. Background maintenance can decay or reflect over older information.

**Inputs:** Activity and stored memories. **Outputs:** Relevant prompt context and updated memory records. **Side effects:** SQLite writes and, for reflection, possible LLM calls.

**Tests:** Memory module tests and `src/db/tests.rs`.

## 5. Connect to an existing session remotely
**Trigger:** Remote pairing and connection commands or mobile client interaction.

**Modules:** `src/app/remote.rs`, `src/runtime/remote.rs`, `src/shim/pump.rs`, `proto/`, `mobile/`.

**Flow:** An authorised peer connects to the remote endpoint. Terminal output is relayed to the client, while input and resize operations are directed to the wrapped session.

**Inputs:** Peer identity, session selection, terminal I/O. **Outputs:** Remote terminal stream. **Side effects:** Network connection and live terminal interaction.

**Tests:** Inspect remote and protocol unit tests before changing authentication or session-control logic.
