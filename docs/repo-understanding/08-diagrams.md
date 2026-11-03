# Architecture diagrams

## Runtime
```mermaid
flowchart LR
  Terminal --> Shim[nsh shim and PTY]
  Shim --> Shell[Interactive shell]
  Shell --> Hooks[Shell hooks]
  Hooks --> Core[nsh-core]
  Shim --> Daemon[Global daemon]
  Core --> Daemon
  Daemon --> DB[(SQLite)]
  Core --> Query[Query engine]
  Query --> Context[Context and memory]
  Context --> DB
  Query --> Provider[LLM provider]
  Provider --> Query
  Query --> Tools[Tool runtime]
  Tools --> Shell
```

## Module relationships
```mermaid
flowchart TD
  App[src/app.rs and cli.rs] --> Query[src/query/]
  App --> Runtime[src/runtime/]
  App --> Config[src/config.rs]
  Query --> Context[src/context/]
  Query --> Providers[src/provider/]
  Query --> Tools[src/tools/]
  Context --> Memory[src/memory/]
  Runtime --> Database[src/db/]
  Memory --> Database
  Tools --> Security[src/security.rs]
  ShellHooks[shell/nsh.*] --> Runtime
  Shim[src/shim/] --> Runtime
```

## Natural-language query
```mermaid
sequenceDiagram
  participant User
  participant Shell as Shell hooks
  participant Core as nsh-core
  participant DB as Daemon / SQLite
  participant LLM as Provider
  participant Tools as Tool runtime
  User->>Shell: ? request
  Shell->>Core: query with session identity
  Core->>DB: load history and relevant memory
  DB-->>Core: stored context
  Core->>LLM: prompt and tool definitions
  loop Until final response or loop limit
    LLM-->>Core: tool call or answer
    opt Tool call
      Core->>Tools: invoke with policy
      Tools-->>Core: result
      Core->>LLM: tool result
    end
  end
  Core-->>Shell: answer or pending command
  Shell-->>User: display or editable prefill
```

These diagrams intentionally omit individual IPC messages, background workers and platform-specific capture details.
