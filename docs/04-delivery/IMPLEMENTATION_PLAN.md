# Implementation Plan

## Delivery model

One milestone at a time. Each milestone uses:
1. build against the shared Acceptance Contract;
2. builder self-audit;
3. independent read-only audit at exact SHA;
4. remediation if needed;
5. re-audit;
6. merge only after approval.

Do not batch several milestones into one giant autonomous build.

## Phase map

| Milestone | Name | Depends on | Outcome |
|---|---|---|---|
| M0 | Engineering foundation | approved docs | runnable secure desktop skeleton |
| M1 | Jobs, briefs, assets, profiles | M0 | persistent print-job domain |
| M2 | Print Engine | M1 | deterministic scale/PPI/preflight core |
| M3 | Astra/OpenAI | M0-M2 | structured conversational planning |
| M4 | Tool protocol + simulator | M2-M3 | safe host-independent design execution |
| M5 | Photoshop adapter | M4 + OI-001 | real Photoshop execution |
| M6 | Illustrator adapter | M4 + OI-001/OI-004 | real Illustrator execution |
| M7 | End-to-end design/revision | M5/M6 | usable design loop |
| M8 | Production export/preflight | M7 + OI-002 | validated print package |
| M9 | Canva (optional) | approval OI-005 | optional Canva workflow |
| M10 | Reference library (optional) | sample data OI-011 | agency style/reference retrieval |
| M11 | Hardening/installer | M8 | release candidate |
| M12 | UAT/release | M11 + production inputs | signed-off production release |

## M0 — Engineering foundation

Suggested implementation:
- initialize pnpm workspace;
- Electron app split main/preload/renderer;
- React UI shell;
- typed secure IPC;
- config service;
- SQLite + migrations;
- secret storage abstraction;
- structured logger/redaction;
- CI;
- unit/integration test setup;
- developer scripts.

Do not integrate Adobe yet.

## M1 — Product/domain foundation

Implement:
- Job list/detail;
- JobBrief form/state;
- PrintSpec;
- unit-safe physical dimensions;
- Asset import/copy/checksum;
- PrinterProfile editor/versioning;
- workspace folders;
- audit service;
- simple roles policy.

## M2 — Print Engine

Implement as a pure/shared package first:
- units;
- scale;
- PPI;
- profile validation;
- preflight findings;
- override policy.

Prefer pure functions and exhaustive tests before UI.

## M3 — Astra

Implement:
- OpenAI provider;
- model configuration;
- job-scoped conversation;
- brief extraction;
- DesignSpec JSON schema;
- planning prompts;
- provider/tool-call validation.

Start with fake/mock provider tests; live smoke test only after secret storage works.

## M4 — Tool simulator

Before Adobe automation:
- formalize tool protocol;
- implement simulator/in-memory design host;
- exercise create/edit/render/save/export;
- complete a fake business-card and fake banner workflow.

This de-risks orchestration separately from vendor APIs.

## M5 — Photoshop

Build Photoshop adapter around the minimum command set required by real test workflows. Do not attempt complete Photoshop coverage.

First vertical slice:
- connect/handshake;
- create document;
- place image;
- create/update text;
- transform;
- render preview;
- save source;
- export supported output.

Add commands only when a product workflow requires them.

## M6 — Illustrator

First resolve OI-004 through a compatibility spike against Vasin's actual version.

Then implement same minimum vertical slice using the approved adapter mechanism. Favor Illustrator for vector-heavy work when capabilities are reliable.

## M7 — Orchestration

Connect:
brief → DesignSpec → plan approval → tool commands → preview → revision.

Add bounded visual critique and element-level delta revisions.

## M8 — Production

Integrate host inspection with Print Engine.
Generate:
- source;
- requested print export;
- preview;
- job-ticket JSON;
- preflight report;
- checksums/manifest.

Real PrinterProfiles from Vasin become mandatory before production sign-off.

## M9/M10 — Optional expansion

Only activate after explicit approval. They must not delay V1 if Vasin's core need is satisfied by Adobe production workflows.

## M11 — Release engineering

- installer;
- signing;
- versioning;
- auto-update decision;
- backup/restore;
- crash recovery;
- compatibility matrix;
- support logs;
- full security/regression test.

## M12 — UAT

Use real Vasin staff, real workstation software versions, approved test assets, and actual print-production settings.

UAT evidence should include screenshots/previews and the files the print operator actually validated, not only unit-test output.

## Branch convention

Suggested:
- `feature/m0-foundation`
- `feature/m1-job-domain`
- ...

Audits target exact commit SHA. Remediation stays on the same milestone branch until the contract passes.

## Definition of done

"Code exists" is not done.
"CI green" is not done.
A milestone is done when the Acceptance Contract passes independent audit at the exact candidate SHA.
