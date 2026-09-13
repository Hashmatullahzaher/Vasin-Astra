# System Architecture

## 1. Architectural goals

Vasin Astra must:
- run as an installable Windows desktop application;
- keep core project files local by default;
- use cloud AI only through explicit provider adapters;
- communicate safely with installed creative applications;
- treat print calculations/preflight as deterministic domain logic;
- remain extensible to new creative tools without rewriting the product core;
- support large-format production without forcing giant flattened bitmaps;
- retain evidence for every production-relevant step.

## 2. Recommended V1 stack

### Desktop
- Electron
- React
- TypeScript
- Vite
- Node.js privileged main/service layer
- pnpm workspaces + Turborepo (or equivalent)

### Local persistence
- SQLite
- migration system
- schema validation with Zod or equivalent
- structured filesystem workspace for native design files/assets/exports

### AI
- provider abstraction
- initial OpenAI Responses API-compatible implementation
- separate image-generation provider interface

### IPC
- Electron secure IPC between renderer and main
- loopback-only authenticated bridge for host plugins/adapters
- adapter-specific communication hidden behind interfaces

## 3. High-level components

```text
┌───────────────────────────────────────────────────────────────┐
│                      Vasin Astra Desktop                      │
│                                                               │
│  ┌──────────────────┐       secure IPC       ┌──────────────┐ │
│  │ React Renderer   │ <--------------------> │ Electron Main│ │
│  │ UI / chat / jobs │                         │ App Service  │ │
│  └──────────────────┘                         └──────┬───────┘ │
│                                                     │         │
│              ┌──────────────────────────────────────┼──────┐  │
│              │                                      │      │  │
│       ┌──────▼──────┐  ┌──────────────┐  ┌────────▼────┐ │  │
│       │ Agent Core  │  │ Print Engine │  │ Persistence │ │  │
│       │ Astra       │  │ + Preflight  │  │ SQLite/files│ │  │
│       └──────┬──────┘  └──────────────┘  └─────────────┘ │  │
│              │                                              │  │
│       ┌──────▼──────────────────────────────────────────┐   │  │
│       │ Tool / Integration Bus (typed commands only)   │   │  │
│       └──────┬───────────────┬───────────────┬──────────┘   │  │
└──────────────┼───────────────┼───────────────┼──────────────┘
               │               │               │
     loopback  │               │               │ OAuth/HTTPS
               ▼               ▼               ▼
       Photoshop UXP      Illustrator       Canva APIs
          adapter            adapter          (optional)
               │               │
               └──── native editable documents ────────────────

Cloud:
- OpenAI model API
- image generation API
- optional Adobe/Canva cloud APIs
```

## 4. Monorepo target layout

```text
/
├─ apps/
│  └─ desktop/
│     ├─ src/renderer/
│     ├─ src/main/
│     └─ src/preload/
├─ integrations/
│  ├─ photoshop-uxp/
│  ├─ illustrator-bridge/
│  └─ canva/
├─ packages/
│  ├─ agent-core/
│  ├─ ai-provider-openai/
│  ├─ design-schema/
│  ├─ print-engine/
│  ├─ tool-protocol/
│  ├─ persistence/
│  ├─ security/
│  ├─ shared/
│  └─ ui/
├─ docs/
├─ prompts/
├─ templates/
└─ tooling/
```

Do not create placeholder packages that have no milestone use. The layout is a target, not an obligation to generate empty folders.

## 5. Trust boundaries

### Renderer
Untrusted relative to OS privileges.
- no API secrets;
- no direct filesystem arbitrary access;
- no process spawning;
- calls allow-listed IPC endpoints.

### Electron main / application service
Privileged.
- performs authorization;
- secret retrieval;
- workspace filesystem access;
- database writes;
- adapter orchestration;
- provider calls.

### Host adapters
Privileged within the design application.
- accept authenticated typed operations;
- report exact host/application version and capability set;
- must reject unknown commands.

### Cloud providers
External.
- receive only required context/assets;
- provider-specific credentials never exposed to renderer or host document.

## 6. Key architectural patterns

### A. DesignSpec as neutral intermediate representation
Astra first creates a tool-independent `DesignSpec`.
Adapters translate supported portions into Photoshop/Illustrator/Canva operations.

### B. PrintSpec independent from DesignSpec
Physical production truth is stored separately so creative edits cannot accidentally change final print intent.

### C. Capability negotiation
Each adapter exposes:
- host/version;
- supported command schema versions;
- supported export formats;
- feature flags.

Astra plans only operations the active adapter reports as supported.

### D. Operation IDs
Every external tool command has:
- operation_id;
- job_id;
- revision_id;
- command type;
- validated payload;
- expected preconditions.

This supports retry safety and audit.

### E. Preview loop
After meaningful tool execution:
1. adapter renders preview;
2. Astra can inspect preview;
3. user reviews;
4. revisions create a new revision rather than destroying history.

### F. Print certification separated from design generation
A beautiful preview is not equivalent to a valid print package.

## 7. Local workspace structure

Suggested per job:

```text
<workspace>/
  jobs/<job-id>/
    assets/
      originals/
      generated/
      derived/
    source/
    previews/
    exports/
    reports/
    metadata/
```

Paths in the database should be normalized and validated to remain within the job workspace unless an explicit import/export operation is authorized.

## 8. Failure handling

- Provider failure: preserve plan/job; retry with bounded policy.
- Plugin disconnect: mark command failed/unknown; never claim success.
- Host crash: reconnect and reconcile document/job state.
- Database write failure: do not continue external destructive operations.
- Export failure: no print-ready state.
- Preflight failure: preserve findings and offer correction path.
- Partial tool command: adapter must return structured outcome and changed object IDs when possible.

## 9. Future architecture extensions

- cloud sync/control plane;
- multi-workstation shared jobs;
- licensing;
- remote audit;
- central reference-design library;
- queue workers for heavy image processing;
- direct RIP/printer integration;
- voice/realtime UI.

These are not V1 blockers.
