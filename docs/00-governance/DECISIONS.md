# Decisions

This is the lightweight ADR register. Detailed ADR files may be added under `docs/00-governance/adr/` as decisions mature.

## D-001 — Windows desktop, local-first
**Status:** ACCEPTED FOR FOUNDATION

The main application will be an installable Windows desktop app. Local design files remain local by default. Cloud services are invoked only for capabilities that need them.

## D-002 — Astra is provider-abstracted
**Status:** ACCEPTED FOR FOUNDATION

"Astra" is the product agent. It must not be coupled to one fixed model ID. The initial provider is OpenAI through a provider adapter.

## D-003 — Deterministic print engine separate from LLM
**Status:** ACCEPTED FOR FOUNDATION

Physical dimensions, scaling, PPI, bleed, safe zones, file eligibility, and preflight are deterministic domain logic. The LLM may propose values but may not bypass validation.

## D-004 — Typed tool execution; no arbitrary generated code
**Status:** ACCEPTED FOR FOUNDATION

Astra invokes typed, allow-listed tool commands. Raw AI-generated OS/script code is never executed in production.

## D-005 — Vector-first production
**Status:** ACCEPTED FOR FOUNDATION

Text, logos, line art, shapes, and layout geometry remain editable/vector wherever the host workflow permits. AI image generation is primarily for raster visual assets, not for flattening the entire finished layout.

## D-006 — Photoshop uses an in-host bridge
**Status:** ACCEPTED FOR FOUNDATION

Use Adobe's current Photoshop extensibility APIs via a dedicated in-host plugin/adapter and a secure loopback bridge to the desktop app.

## D-007 — Illustrator behind an adapter boundary
**Status:** ACCEPTED FOR FOUNDATION

Because Illustrator automation options vary by environment and API availability, the core never depends directly on one automation technology. A stable `IllustratorAdapter` interface isolates local scripting or Adobe API implementations.

## D-008 — Canva is optional, not the print-production source of truth
**Status:** ACCEPTED FOR FOUNDATION

Canva may be integrated for template/design workflows, but print-ready certification comes from Vasin Astra's print engine and supported production export path.

## D-009 — Local persistence
**Status:** ACCEPTED FOR FOUNDATION

V1 uses a local structured database plus project file workspace. A future cloud sync/control plane must not be required to complete core local print workflows.

## D-010 — Suggested implementation stack
**Status:** PROVISIONAL

- Electron
- React
- TypeScript
- Vite
- Node.js main/service layer
- SQLite
- Zod or equivalent schema validation
- pnpm workspace/Turborepo

This stack is chosen for mature Windows packaging and direct local integration. It may be changed only through an explicit architecture decision with equivalent capabilities and migration rationale.
