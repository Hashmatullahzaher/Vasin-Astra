# Product Requirements — Vasin Astra

## 1. Product statement

Vasin Astra is a Windows desktop application that lets a printing-agency operator collaborate conversationally with an AI agent to create, revise, validate, and export professional print designs using connected creative tools.

The product must behave as a **design-production system**, not merely a prompt-to-image UI.

## 2. Primary V1 workflow

1. Operator creates a print job/project.
2. Operator enters or chats a brief.
3. Operator selects/attaches logos, images, text, brand assets, and references.
4. Operator chooses a print product/profile and final physical size.
5. Astra extracts missing facts and creates a structured design intent.
6. Print Engine validates production feasibility and chooses/validates document scale.
7. Astra proposes a design plan and target host tool.
8. Operator approves execution where policy requires.
9. Astra invokes typed tool operations through the relevant adapter.
10. Host tool produces/updates an editable design.
11. Astra obtains a preview and visually evaluates it.
12. Operator requests revisions in natural language.
13. Astra edits the existing design rather than unnecessarily regenerating from scratch.
14. Operator requests finalization.
15. Preflight validates the design against the selected printer profile.
16. Hard failures are corrected or explicitly overridden by an authorized user.
17. Astra exports the editable source, requested print file(s), preview, production metadata, and preflight report.
18. Job history records revisions, tool calls, exports, and decisions.

## 3. Functional requirements

### FR-001 Project/job management
Create, rename, duplicate, archive, and reopen jobs. Each job has a unique ID and workspace folder.

### FR-002 Structured print brief
Capture:
- product type;
- customer/job name;
- final width/height and unit;
- orientation;
- quantity where relevant;
- substrate/material;
- viewing context/distance if relevant;
- printer profile;
- copy/text;
- logos/assets;
- brand colors/fonts;
- preferred style;
- required output formats;
- finishing notes;
- deadline/notes.

### FR-003 Conversational Astra
Astra must:
- accept natural-language instructions;
- inspect attached images/previews;
- identify missing high-impact facts;
- explain production warnings in plain language;
- plan before modifying external tools;
- maintain context per job/revision.

### FR-004 Provider abstraction
OpenAI is the initial AI provider. Model IDs/configuration must be changeable without rewriting the product domain.

### FR-005 Design-spec generation
Before execution, Astra creates a validated structured design spec containing:
- final print specification;
- working-document scale;
- canvas/artboard setup;
- element list;
- z-order;
- typography;
- colors;
- asset references;
- vector/raster intent;
- tool target;
- output intent.

### FR-006 Photoshop integration
The app can connect to a Photoshop-side adapter, verify connection/host version, create/open a design, modify supported elements, export previews, save editable source, and report errors.

### FR-007 Illustrator integration
The app can connect through the selected Illustrator adapter implementation and perform the equivalent vector-oriented production workflow for supported operations.

### FR-008 Canva integration
Optional Phase 2 capability. The adapter may create/sync/export Canva designs subject to Canva APIs and account permissions. Canva is not required to certify huge print output.

### FR-009 AI image generation
Astra may generate visual assets/backgrounds through an image model. Generated imagery must be treated as a raster asset and evaluated for final effective resolution. It must not silently replace editable text/logo/layout with a flattened image.

### FR-010 Print profile management
Admin can create/edit/duplicate profiles containing production constraints and output expectations.

### FR-011 Scale-aware large format
Support physical sizes larger than practical working-document sizes by representing final size separately from document scale and calculating effective resolution at 1:1.

### FR-012 Raster resolution validation
For every raster placement, calculate effective final-size PPI and compare it to profile thresholds.

### FR-013 Preflight
At minimum validate:
- final dimensions;
- document scale;
- bleed/safe-zone configuration;
- raster effective PPI;
- missing/invalid assets;
- unsupported color mode/profile where detectable;
- missing fonts where detectable;
- text/logo outside safe zone;
- required export format;
- document/source availability;
- profile-required checks.

### FR-014 Revision history
Each meaningful design revision has an ID, timestamp, instruction, tool execution summary, and associated preview.

### FR-015 Print package export
Produce a predictable job folder/package with:
- editable source;
- print-ready export(s);
- preview;
- preflight report;
- machine-readable job ticket/metadata;
- optional linked assets.

### FR-016 Audit log
Record security/production-relevant actions, including preflight overrides and destructive file operations.

### FR-017 Settings/integrations
Settings must show:
- OpenAI connection;
- Photoshop connection;
- Illustrator connection;
- Canva connection when enabled;
- workspace path;
- printer profiles;
- application/update information.

### FR-018 Secure credential storage
Sensitive credentials are encrypted using OS-backed secret storage, not plaintext files.

## 4. User experience requirements

- The operator should not need programming knowledge.
- Use physical units familiar to print shops.
- Always display final size and working scale together when scale is not 1:1.
- Production warnings must distinguish **BLOCKER**, **WARNING**, and **INFO**.
- Show what tool Astra is about to use and what it intends to change.
- Preserve a visible undo/revision strategy.
- Avoid silent destructive overwrites.

## 5. Performance/reliability requirements

- App cold start target: reasonable on current business Windows hardware; exact target established during performance baselining.
- Local job list and preflight should work without cloud connectivity when all required local data exists.
- A failed design-tool call must not corrupt job metadata.
- Tool commands must be idempotent or carry operation IDs where feasible.
- Long-running image/API/tool jobs must expose progress and cancellation where feasible.
- Autosave job metadata; never imply a host document was saved unless confirmed by the adapter.

## 6. Production quality requirements

- Print-readiness is a state reached only after profile-based preflight.
- PPI must be evaluated at final size.
- Vector assets must not be rasterized without a justified workflow reason.
- The system must support scaled large-format documents.
- Export metadata must preserve final physical dimensions even for scaled documents.
