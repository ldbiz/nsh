# Tests and fixtures

## Framework and invocation
The repository uses Rust's built-in test harness (`cargo test`), with Tokio asynchronous tests and supporting `serial_test` and `wiremock` dependencies. `tests/integration.rs` exercises the built `nsh-core` binary.

The integration harness creates temporary home directories and clears selected environment settings to isolate state. This is more representative of a CLI installation than tests calling internal functions alone.

## Important coverage
| Location | Behaviour covered |
|---|---|
| `tests/integration.rs` | Shell init, CLI help/config/status, history, recording, daemon, memory and diagnostics |
| `src/shim_main.rs` tests | Managed core resolution and shim environment contract |
| `src/shim/` tests | Wrapper selection and terminal capture logic |
| `src/query/mod.rs` tests | Tool-loop and provider interaction using mocks |
| `src/query/prompt.rs` tests | Prompt context and platform-specific guidance |
| `src/context/tests.rs` | Context extraction and formatting |
| `src/db/tests.rs` | Persistence and history/memory behaviour |
| `src/security.rs` tests | Command hazard patterns and safety classification |
| `src/tools/tests.rs` | Tool execution and restrictions |
| `src/app/runtime.rs` tests | Update metadata parsing and selection |

## Fixtures
The most important test fixtures are temporary home/database environments, mock provider responses, and synthetic command/session/context data. There is no separate top-level fixtures directory in the inspected repository tree.

## Running
```bash
cargo test
```

For more focused work, run a specific Rust test module or integration target. The remote feature is enabled by default and may affect compilation prerequisites.

## Limits
This pack was produced from static repository inspection. Tests were not executed here. End-to-end interactive PTY behaviour, real provider calls, remote connections, platform differences and shell startup combinations still need live validation.
