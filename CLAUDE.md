# Claude Code — Project Map

## How to Build & Run

```bash
bun install          # install dependencies
bun run build        # bundles to dist/cli.js (~23MB)
bun dist/cli.js      # run it
```

## Project Structure

```
claude-code/
├── dist/                          # Build output (gitignored)
│   └── cli.js                     # Bundled CLI (23MB, single file)
│
├── src/                           # Main source (1,929 files) — leaked from Anthropic
│   ├── main.tsx                   # CLI entrypoint — Commander.js parser, all flags
│   ├── entrypoints/
│   │   ├── cli.tsx                # Bootstrap — version check, fast-paths
│   │   ├── init.ts                # Initialization — telemetry, config, auth
│   │   ├── mcp.ts                 # MCP server entrypoint
│   │   └── sdk/                   # Agent SDK types
│   │       ├── coreSchemas.ts     # Zod schemas (source of truth for types)
│   │       ├── coreTypes.ts       # Re-exports generated types
│   │       ├── coreTypes.generated.ts  # [GENERATED] from coreSchemas.ts
│   │       ├── runtimeTypes.ts    # [STUB] SDK runtime types
│   │       ├── toolTypes.ts       # [STUB] SDK tool types
│   │       └── settingsTypes.generated.ts  # [STUB] Settings types
│   │
│   ├── commands/                  # Slash commands (~50)
│   │   ├── agents-platform/       # [STUB] Ant-only
│   │   └── assistant/             # [STUB] Assistant wizard
│   │
│   ├── tools/                     # Agent tools (~40)
│   │   ├── BashTool/              # Shell execution
│   │   ├── FileEditTool/          # File editing
│   │   ├── FileReadTool/          # File reading
│   │   ├── FileWriteTool/         # File writing
│   │   ├── GlobTool/              # File search
│   │   ├── GrepTool/              # Content search
│   │   ├── AgentTool/             # Subagent spawning
│   │   ├── WebFetchTool/          # HTTP fetching
│   │   ├── TungstenTool/          # [STUB] Ant-only debug tool
│   │   ├── REPLTool/              # [STUB] Ant-only REPL
│   │   ├── SuggestBackgroundPRTool/ # [STUB] Ant-only
│   │   ├── VerifyPlanExecutionTool/ # [STUB] Env-gated
│   │   └── WorkflowTool/          # [STUB] Feature-gated (WORKFLOW_SCRIPTS)
│   │
│   ├── components/                # React (Ink) UI components (~140)
│   │   ├── agents/
│   │   │   └── SnapshotUpdateDialog.tsx  # [STUB]
│   │   ├── design-system/         # Theme, colors, tokens
│   │   ├── LogoV2/                # Welcome screen, release notes
│   │   ├── Message.tsx            # Message rendering
│   │   ├── StructuredDiff/        # Syntax-highlighted diffs
│   │   └── permissions/           # Permission approval dialogs
│   │
│   ├── screens/
│   │   └── REPL.tsx               # Main interactive screen (2800+ lines)
│   │
│   ├── ink/                       # Custom Ink fork (terminal React renderer)
│   │   ├── layout/                # Flexbox layout engine
│   │   ├── components/            # Box, Text, ScrollBox, Button, etc.
│   │   ├── hooks/                 # useInput, useStdin, useSelection, etc.
│   │   ├── events/                # Click, keyboard, focus events
│   │   ├── termio/                # Terminal I/O, ANSI parsing
│   │   └── reconciler.ts          # React reconciler
│   │
│   ├── services/
│   │   ├── api/                   # Anthropic API client, streaming, errors
│   │   ├── mcp/                   # MCP client/server implementation
│   │   ├── oauth/                 # OAuth flow
│   │   ├── analytics/             # Telemetry, GrowthBook, DataDog
│   │   ├── lsp/                   # Language Server Protocol
│   │   ├── compact/               # Context compaction
│   │   │   ├── snipCompact.ts     # [STUB] Feature-gated (HISTORY_SNIP)
│   │   │   └── cachedMicrocompact.ts  # [STUB] Feature-gated
│   │   └── contextCollapse/       # [STUB] Not in leak
│   │
│   ├── native-ts/                 # Pure TypeScript ports of native modules
│   │   ├── yoga-layout/           # Flexbox engine (port of Meta's Yoga)
│   │   ├── color-diff/            # Syntax-highlighted diffs (port of Rust module)
│   │   └── file-index/            # Fuzzy file search (port of nucleo)
│   │
│   ├── constants/
│   │   ├── prompts.ts             # FULL system prompt — the actual instructions sent to Claude
│   │   ├── oauth.ts               # OAuth config (client IDs, endpoints)
│   │   └── product.ts             # Product constants
│   │
│   ├── utils/
│   │   ├── autoUpdater.ts         # Version check [PATCHED — remote check disabled]
│   │   ├── computerUse/           # Computer use integration layer
│   │   ├── claudeInChrome/        # Chrome integration layer
│   │   ├── sandbox/               # Sandbox adapter
│   │   ├── settings/              # Settings system
│   │   ├── model/                 # Model selection, aliases
│   │   ├── auth.ts                # Authentication
│   │   ├── protectedNamespace.ts  # [STUB] Ant-only
│   │   └── filePersistence/
│   │       └── types.ts           # [STUB]
│   │
│   ├── assistant/
│   │   ├── sessionHistory.ts      # Session history
│   │   └── AssistantSessionChooser.tsx  # [STUB]
│   │
│   ├── vim/                       # Vim mode (motions, operators, text objects)
│   ├── state/                     # App state management
│   ├── hooks/                     # React hooks
│   ├── types/
│   │   └── connectorText.ts       # [STUB]
│   ├── bridge/                    # Cloud session bridging
│   ├── coordinator/               # Multi-agent coordinator
│   ├── plugins/                   # Plugin system
│   ├── skills/                    # Built-in skills
│   │   └── bundled/verify/        # [STUB] Placeholder .md files
│   ├── bootstrap/                 # Bootstrap/startup state
│   └── voice/                     # Voice mode
│
├── stubs/                         # Extracted proprietary source code
│   ├── @ant/                      # Private Anthropic packages (28 files)
│   │   ├── computer-use-mcp/      # Computer Use MCP server
│   │   │   └── src/
│   │   │       ├── index.ts       # Exports
│   │   │       ├── toolCalls.ts   # 137KB — full tool implementation
│   │   │       ├── tools.ts       # Tool definitions
│   │   │       ├── mcpServer.ts   # MCP server setup
│   │   │       ├── types.ts       # All CU types
│   │   │       ├── deniedApps.ts  # App blocklist
│   │   │       ├── keyBlocklist.ts # Key combo blocklist
│   │   │       ├── sentinelApps.ts # Sentinel app detection
│   │   │       ├── imageResize.ts # Screenshot resizing
│   │   │       ├── pixelCompare.ts # Click target validation
│   │   │       ├── executor.ts    # [STUB] Native Swift/Rust bridge interface
│   │   │       └── subGates.ts    # [STUB] Permission sub-gates
│   │   │
│   │   ├── claude-for-chrome-mcp/ # Chrome automation (8 source files)
│   │   │   └── src/
│   │   │       ├── index.ts       # Exports
│   │   │       ├── bridgeClient.ts # 37KB — Chrome bridge
│   │   │       ├── browserTools.ts # 25KB — browser tool definitions
│   │   │       ├── mcpServer.ts   # MCP server
│   │   │       ├── mcpSocketClient.ts # WebSocket client
│   │   │       ├── mcpSocketPool.ts   # Connection pooling
│   │   │       ├── toolCalls.ts   # Tool call handling
│   │   │       └── types.ts       # Types
│   │   │
│   │   ├── computer-use-swift/    # macOS native bridge
│   │   │   └── js/index.js        # JS loader for Swift binary
│   │   │
│   │   └── computer-use-input/    # Input device bridge
│   │       └── js/index.js        # JS loader for Rust binary
│   │
│   └── @anthropic-ai/            # Anthropic SDK sources (105 files)
│       ├── sandbox-runtime/       # Sandbox system (14 files)
│       │   └── dist/
│       │       ├── sandbox/
│       │       │   ├── sandbox-manager.js    # Core sandbox manager
│       │       │   ├── sandbox-config.js     # Config/schema
│       │       │   ├── macos-sandbox-utils.js # macOS sandbox profiles
│       │       │   ├── linux-sandbox-utils.js # Linux seccomp/namespaces
│       │       │   ├── generate-seccomp-filter.js # Seccomp BPF generator
│       │       │   ├── http-proxy.js         # HTTP egress proxy
│       │       │   ├── socks-proxy.js        # SOCKS proxy
│       │       │   └── sandbox-violation-store.js
│       │       └── utils/
│       │
│       ├── mcpb/                  # MCP Bundle tools (11 files)
│       │   └── dist/
│       │       ├── cli/           # Pack/unpack/init CLI
│       │       ├── node/          # File handling, signing, validation
│       │       └── shared/        # Config, logging
│       │
│       ├── sdk/                   # Anthropic SDK source (40+ files)
│       │   ├── client.mjs         # Main client
│       │   ├── resources/         # API resources (messages, models, batches)
│       │   ├── lib/               # Streaming, tool runners, parsers
│       │   └── internal/          # Headers, auth, request handling
│       │
│       ├── bedrock-sdk/           # AWS Bedrock (12 files)
│       ├── vertex-sdk/            # GCP Vertex (7 files)
│       └── foundry-sdk/           # Foundry (8 files)
│
├── shims/                         # Build-time shims
│   ├── bun-bundle.ts              # Runtime shim for feature() — returns false
│   ├── bun-bundle.d.ts            # Type declaration
│   └── globals.d.ts               # MACRO.* type declarations
│
├── scripts/
│   └── generate-sdk-types.ts      # Generates coreTypes.generated.ts from Zod schemas
│
├── vendor/                        # Native binaries from npm package (gitignored)
│   ├── ripgrep/                   # rg binary (arm64/x64 for darwin/linux/win32)
│   └── audio-capture/             # Voice capture native addon (all platforms)
│
├── build.ts                       # Bun build script
├── package.json                   # Dependencies & scripts
├── tsconfig.json                  # TypeScript config
├── bun.lock                       # Bun lockfile
├── .gitignore
├── LICENSE                        # MIT
├── README.md
│
├── cli.js.map                     # Original 57MB source map (gitignored, saved locally)
└── sourcemap-extract.tar.gz       # Full extraction archive (gitignored, saved locally)
```

## What's Patched

- `src/utils/autoUpdater.ts` — remote version check disabled (line 72: early return)
- `build.ts` — MACRO.VERSION set to `2.1.88`, all feature() flags return false

## What's Stubbed (marked [STUB] above)

Files that exist but contain minimal placeholder code because:
1. **Not in leak** — source files excluded from the original zip
2. **Native bindings** — Rust/Swift code can't be in a source map (executor.ts, subGates.ts)
3. **Generated files** — were generated by build scripts (coreTypes.generated.ts — we regenerated this)
4. **Ant-only** — internal Anthropic tools gated by `USER_TYPE === 'ant'`

## Feature Flags (all disabled)

The source uses `feature('FLAG_NAME')` from `bun:bundle` for dead code elimination.
Our shim returns `false` for all flags. Known flags:
VOICE_MODE, COORDINATOR_MODE, KAIROS, PROACTIVE, ULTRAPLAN, BRIDGE_MODE,
BG_SESSIONS, WORKFLOW_SCRIPTS, TRANSCRIPT_CLASSIFIER, TOKEN_BUDGET,
HISTORY_SNIP, BUDDY, TEAMMEM, AGENT_TRIGGERS, WEB_BROWSER_TOOL,
MESSAGE_ACTIONS, HOOK_PROMPTS, CACHED_MICROCOMPACT, CHICAGO_MCP,
ABLATION_BASELINE, DUMP_SYSTEM_PROMPT
