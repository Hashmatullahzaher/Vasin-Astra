# Business Rules

## BR-001 Final size is mandatory
A job cannot enter design execution without explicit final width, final height, and physical unit.

## BR-002 Final size and document size are different concepts
When scale != 1:1, both must be stored and displayed.

## BR-003 Profile controls print rules
No universal PPI, bleed, scale, color, or export preset may override an explicitly selected PrinterProfile.

## BR-004 PPI is measured at final placed size
Raster quality decisions use effective final-size PPI.

## BR-005 Vector-first
Text/logos/shapes remain editable/vector whenever supported.

## BR-006 AI-generated images are assets, not automatically finished designs
Generated raster output must pass the same resolution and placement checks as any other raster source.

## BR-007 No false execution claims
A tool action is successful only after the adapter confirms it.

## BR-008 No false save/export claims
A source/output is saved only after filesystem/adapter confirmation and metadata persistence.

## BR-009 Preflight before print-ready
No artifact/job may be labeled print-ready without completed profile-based preflight.

## BR-010 Blockers stop certification
Unresolved BLOCKER findings prevent print-ready export/certification.

## BR-011 Override is explicit
If profile policy permits override:
- authorized user only;
- reason required;
- affected finding IDs recorded;
- audit event required;
- report marks the override.

## BR-012 Missing production facts are not guessed
If an official Vasin rule is unknown, mark the dependent condition OPEN/MANUAL_CHECK and request clarification.

## BR-013 Host tool selection is capability-driven
Astra may propose Photoshop, Illustrator, or later Canva based on the job, but the print spec remains tool-independent.

## BR-014 Existing design revisions should be edited, not regenerated unnecessarily
When user requests a bounded change to an existing design, preserve unchanged elements and revision lineage.

## BR-015 Tool commands are typed
Production adapters accept validated command schemas. Arbitrary model-generated executable code is forbidden.

## BR-016 File safety
Project files remain within the job workspace unless the operator explicitly imports/exports elsewhere.

## BR-017 Destructive actions require confirmation
Delete/overwrite of user-owned files requires a clear confirmation or a documented safe versioning strategy.

## BR-018 PrinterProfile version is pinned to exported package
A print package stores the exact profile version used for preflight.

## BR-019 Scale conversion must preserve physical truth
If a 1:10 document is exported for a printer expecting scale-aware artwork, job ticket/preflight must state both scale and 1:1 final dimensions.

## BR-020 Large-format raster feasibility warning
The system must identify impractical raster dimensions/memory demands and prefer vector + placed-image composition or approved scaled workflows instead of blindly allocating a massive 1:1 bitmap.

## BR-021 Human-visible change plan
Before a meaningful external-tool modification, UI should show a concise plan unless the operator has explicitly enabled an approved low-risk auto-execute policy.

## BR-022 Audit retention
Security/production-relevant actions are retained with job ID, actor, timestamp, operation, outcome, and relevant artifact/revision references.
