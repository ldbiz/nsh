# Caveats and unknowns

This documentation is based on static inspection of the fork's `main` tree, not a running installation.

## Important boundaries to validate
- **Shell coverage:** Bash, Zsh, Fish and PowerShell integrations differ. Do not infer exact behaviour in one shell from another shell's hook implementation.
- **PTY versus hooks:** The shim captures terminal output; hooks report command metadata. Verify which capture mode and wrapper conditions apply in a given installation.
- **Shim/core compatibility:** The code documents a deliberately stable wrapper boundary, but compatibility across arbitrary historical versions has not been tested here.
- **Execution guarantees:** Risk classification is pattern-based. It is not a proof that a command is harmless. Shell expansions, indirect effects and environment-dependent commands need independent threat review.
- **Provider behaviour:** Model capabilities, tool support, pricing and error handling depend on the selected provider and current external API.
- **Memory quality:** The six-tier architecture exists in code; retrieval usefulness, privacy and lifecycle behaviour require empirical evaluation.
- **Remote security:** Pairing, authorisation and encrypted transport need runtime security testing before relying on them in a sensitive environment.
- **Persistence:** Schema versioning and migrations are visible; upgrade/downgrade compatibility was not exercised.
- **Platform differences:** Unix PTY and Windows terminal paths are not interchangeable.
- **MCP exposure:** `src/mcp_server.rs` exposes a restricted subset; `run_command` still warrants explicit review of its allowlist and risk policy.

## Questions for maintainers
1. Which shell/OS combinations are considered release-tested rather than experimental?
2. What compatibility guarantees are maintained between shim, core and daemon versions?
3. Which security boundaries are intended as hard enforcement versus advisory model guidance?
4. What are the expected retention and privacy guarantees for terminal output and memory?
5. Which interactive PTY, remote and provider flows have repeatable live acceptance tests?

## Validation not performed
- No `cargo test`, live PTY session, LLM request, remote pairing or mobile build was run.
- Mermaid diagrams were authored for GitHub Markdown but not visually rendered in this environment.
- The pack describes the current code; it is not an endorsement of every README claim or a security audit.
