# Core concepts

| Concept | Meaning in nsh | Where to look |
|---|---|---|
| Stable shim | The `nsh` binary owns the wrapping boundary and delegates other commands to a managed core | `src/shim_main.rs`, `src/shim/` |
| Core executable | The separately updated command implementation (`nsh-core`) | `src/core_main.rs`, `src/app.rs` |
| PTY capture | Terminal I/O is observed and retained as scrollback for context | `src/shim/pump.rs` |
| Shell hooks | Shell-specific pre/post-command integration and prefill coordination | `shell/`, `src/app/init.rs` |
| Global daemon | Shared background process for session, history and memory operations | `src/runtime/global_daemon.rs`, `src/runtime/daemon.rs` |
| Session | Identifies a shell/terminal activity stream and its associated history and conversation | `src/app/session.rs`, `src/db/sessions.rs` |
| Query context | Snapshot of environment, terminal, project, history and optional cross-session activity | `src/context/mod.rs`, `src/context/project.rs` |
| Agent tool loop | LLM response can request further tool calls before a final answer | `src/query/mod.rs`, `src/tools/` |
| Command prefill | Suggested command is handed back to the shell for editing or execution | `src/tools/command.rs`, `shell/` |
| Execution mode | Controls review, confirmation and automatic execution paths | `src/config.rs`, `src/tools/command.rs` |
| Persistent memory | Core, episodic, semantic, procedural, resource and encrypted knowledge stores | `src/memory/`, `src/db/memory_ops.rs` |
| Memory ingestion | Converts shell events into durable, selected memories | `src/memory/ingestion/` |
| Memory retrieval | Finds relevant stored context for subsequent queries | `src/memory/retrieval/` |
| Risk assessment | Classifies command hazards and helps gate execution | `src/security.rs`, `src/tools/command.rs`, `src/tools/run_command.rs` |
| Tool result boundary | Marks external tool output as untrusted in model context | `src/security.rs`, `src/query/mod.rs` |
| Remote session | Optional access to terminal sessions from another device | `src/runtime/remote.rs`, `mobile/` |

These concepts describe nsh's existing implementation; they are not recommendations for a different shell assistant.
