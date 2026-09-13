# Acceptance Contract

## 1. Purpose

This is the shared contract for both builders and independent auditors.

A milestone is complete only when implementation, tests, security constraints, and evidence satisfy this document at the exact candidate commit SHA.

The builder may not redefine acceptance after implementation. The auditor may not invent new product requirements after the fact; newly discovered security/integrity defects can still block release.

## 2. Severity

- **SEV-1 Critical:** security/data-loss/production integrity defect with immediate unacceptable risk.
- **SEV-2 High:** core acceptance criterion fails or print-ready claim can be materially wrong.
- **SEV-3 Medium:** important defect or incomplete non-core behavior with workaround.
- **SEV-4 Low:** polish/maintainability issue not blocking milestone unless explicitly required.

No milestone may pass with unresolved SEV-1 or SEV-2 findings.

## 3. Global acceptance rules

Every implementation milestone must satisfy:

### G-01 Exact candidate
- final commit SHA is reported;
- working tree clean;
- audit and CI target that SHA.

### G-02 Scope
- only active milestone implemented;
- later milestones not silently started;
- scope changes documented.

### G-03 Build quality
As applicable:
- install dependencies from lockfile;
- format check;
- lint;
- TypeScript typecheck;
- unit tests;
- integration tests;
- build.

### G-04 Security
- no committed secrets;
- renderer has no privileged Node access;
- no raw LLM-generated executable code path;
- input schemas at trust boundaries;
- safe path handling;
- security tests for touched boundary.

### G-05 Data integrity
- migrations versioned;
- foreign/invariant constraints tested;
- file/database state transitions fail safely.

### G-06 Evidence
Builder provides:
- commands run;
- pass/fail output summary;
- screenshots/recordings for UI/host-tool behavior where applicable;
- produced test artifacts;
- known limitations;
- open items.

### G-07 Documentation
If implementation changes an accepted architecture/domain contract:
- proposal/decision updated before or with code;
- conflicting docs not left stale.

## 4. Milestone acceptance

# M0 — Engineering foundation

Must provide:
- pnpm workspace/monorepo;
- Electron + React + TypeScript desktop shell;
- secure preload/IPC skeleton;
- local application-service boundary;
- SQLite migration framework;
- structured config;
- OS-secret storage abstraction with fake/test provider;
- logging with redaction;
- test framework;
- CI for format/lint/typecheck/test/build;
- Windows development build launches.

Acceptance:
- renderer cannot import Node privileged modules;
- sample secret is stored/retrieved through abstraction and not exposed in renderer/logs;
- database initializes/migrates from clean state;
- CI passes at exact SHA.

# M1 — Jobs, briefs, assets, printer profiles

Must provide:
- Job CRUD/archive;
- JobBrief persistence;
- PrintSpec persistence;
- Asset import into workspace;
- PrinterProfile + immutable/versioned profile configuration;
- settings UI for workspace/profile;
- basic roles/authorization service.

Acceptance:
- path traversal import rejected;
- profile version pinned to Job/PrintSpec;
- physical units round-trip accurately;
- jobs survive restart;
- destructive file operation requires policy confirmation/versioning.

# M2 — Deterministic Print Engine

Must provide:
- unit conversion;
- final vs working dimensions;
- scale rules;
- effective PPI calculator;
- profile thresholds;
- initial preflight engine;
- structured findings;
- override domain rules.

Acceptance:
- property/unit tests cover conversions/invariants;
- 1:1 and scaled large-format cases;
- threshold edge cases;
- missing profile/document mismatch cases;
- no universal hard-coded 300-PPI rule;
- preflight state cannot become PASS with unresolved blocker.

# M3 — Astra agent and OpenAI provider

Must provide:
- provider interface;
- OpenAI implementation;
- secure credential configuration;
- conversation per job;
- structured Brief extraction;
- structured DesignSpec schema;
- tool planning interface;
- provider error/retry behavior;
- token/cost metadata where available.

Acceptance:
- invalid model output cannot cross schema boundary;
- no arbitrary code execution tool;
- API key never reaches renderer/log;
- agent cannot mark print-ready;
- mocked provider tests deterministic;
- live provider smoke test documented separately and does not expose secret.

# M4 — Tool protocol and simulator

Must provide:
- versioned typed ToolCommand/ToolResult schemas;
- capability negotiation;
- operation IDs;
- mock/simulator adapter capable of document/element/preview/export flow;
- adapter contract tests.

Acceptance:
- unknown command rejected;
- unsupported capability rejected before execution;
- duplicate operation handling tested;
- no generic RUN_CODE/RUN_SCRIPT/RAW command;
- an end-to-end mock design workflow creates a revision + preview without Photoshop/Illustrator.

# M5 — Photoshop integration

Must provide:
- Photoshop in-host adapter/plugin;
- authenticated loopback bridge;
- host/version/capability handshake;
- minimum command set needed for first real test product;
- preview render;
- source save;
- supported export;
- structured error/reconnect behavior.

Acceptance:
- unauthenticated local client rejected;
- LLM has no raw batchPlay endpoint;
- real Photoshop test evidence at supported version;
- create → edit → preview → save flow confirmed;
- host disconnect never reported as success.

# M6 — Illustrator integration

Must provide:
- selected/approved Illustrator adapter implementation;
- compatibility evidence against Vasin target version;
- minimum vector-oriented command set;
- preview/save/export;
- reconnect/error behavior.

Acceptance:
- no raw arbitrary-script model path;
- real Illustrator test evidence;
- vector text/logo remains editable in test document;
- create → edit → preview → save flow confirmed.

# M7 — Real design/revision workflow

Must provide:
- target-tool selection;
- design plan review UI;
- design execution orchestration;
- preview display;
- natural-language revision patching;
- revision lineage;
- visual inspection loop with bounded iterations.

Acceptance:
- business-card-class test job completes design loop;
- large-banner-class test job completes scaled design loop;
- bounded change preserves instructed locked/unchanged elements;
- adapter failure produces recoverable error, not false completion.

# M8 — Production preflight and export package

Must provide:
- host inspection mapping into preflight evidence;
- final preflight UI;
- blockers/warnings/manual checks;
- authorized override workflow;
- print package export;
- manifest/job ticket;
- checksums;
- preflight report.

Acceptance:
- source + requested output + preview + metadata + report generated;
- package records final 1:1 dimensions and document scale;
- low-PPI raster test blocks according to profile;
- override leaves visible evidence and audit event;
- profile version mismatch invalidates/requires new preflight;
- no failed export becomes print-ready.

# M9 — Canva adapter (optional / only if approved)

Acceptance contract to be activated only after OI-005 is resolved. Must distinguish Connect API capabilities from Apps SDK in-editor editing. Canva size/API limitations must not be hidden.

# M10 — Reference design library (optional / only if approved)

Must provide import/index/retrieval without silently copying unrelated source work. Acceptance criteria finalized when sample data and scope are approved.

# M11 — Hardening and Windows release candidate

Must provide:
- production installer;
- code-signing plan/implementation when certificates available;
- config migration;
- backup/restore;
- crash recovery;
- dependency/security review;
- update strategy;
- performance baseline;
- production logging/support bundle with redaction.

Acceptance:
- clean Windows install/uninstall test;
- upgrade preserves existing jobs;
- restore test succeeds;
- no secrets in support bundle;
- release build passes full regression.

# M12 — UAT and release

Must complete real operator UAT using Vasin-approved printer profiles and workstation versions.

Required UAT scenarios:
1. close-view small-format job;
2. multi-meter large-format job;
3. at least one natural-language revision;
4. low-resolution asset rejection/warning;
5. adapter disconnect/recovery;
6. export package validation by Vasin production operator;
7. application restart/reopen;
8. backup/restore or recovery scenario.

Release gate:
- UAT signed off;
- no SEV-1/SEV-2;
- all production P0 open items resolved or explicitly waived;
- supported Windows/Adobe versions documented;
- installer verified;
- rollback/recovery documented.

## 5. Auditor decision strings

Use exactly one:
- **PASS — milestone contract satisfied at audited SHA.**
- **PASS WITH NON-BLOCKING FINDINGS — milestone contract satisfied; listed SEV-3/4 remediation may follow.**
- **FAIL — milestone contract not satisfied; remediation required.**
- **BLOCKED — insufficient evidence or external prerequisite prevents a valid decision.**
