# Test Plan

## 1. Test philosophy

Vasin Astra can produce physical print artifacts and control privileged local software. Testing must cover calculations, state, integrations, security, and real operator workflows.

## 2. Test layers

### Unit
- unit conversion;
- scale arithmetic;
- effective PPI;
- preflight decision logic;
- authorization;
- path normalization;
- schema validation;
- job state transitions;
- profile version rules.

### Property-based / invariant
Recommended for:
- unit conversion round trips;
- scale invariants;
- physical-size equivalence;
- PPI monotonicity;
- path/workspace boundaries.

### Integration
- SQLite repositories/migrations;
- filesystem workspace;
- secret-store abstraction;
- renderer ↔ preload ↔ main IPC;
- AI provider mock;
- adapter protocol;
- export package assembly.

### Contract
Every design adapter runs the same adapter contract suite where technically possible:
- capabilities;
- create/open;
- apply operation;
- inspect;
- preview;
- save;
- export;
- disconnect/error.

### UI
Use an appropriate Electron-compatible automation stack (e.g. Playwright Electron support) for:
- new job;
- profile selection;
- settings;
- chat;
- preflight;
- export flow.

### Real-host tests
Separate from pure CI when licensed desktop applications are required.

Photoshop:
- connection;
- supported host/version;
- create/edit/save/preview/export;
- reconnect.

Illustrator:
- same;
- verify vector editability.

### Security
- renderer privilege escape attempts;
- invalid IPC channel/payload;
- secret redaction;
- path traversal;
- localhost unauthenticated access;
- malformed/oversized tool messages;
- raw-code command rejection;
- unauthorized preflight override;
- malicious content/prompt injection scenario.

### Migration
- clean install;
- upgrade from previous schema;
- interrupted/failed migration recovery test as feasible.

### Packaging
- Windows build;
- installer;
- uninstall;
- upgrade;
- signed artifact validation when signing available.

## 3. Required domain fixtures

### Fixture A — close-view small format
Configurable example profile with:
- small physical dimensions;
- high effective PPI expectation;
- bleed/safe zone;
- PDF/native output.

Numbers are test data, not Vasin production defaults.

### Fixture B — large format
Example:
- final 2 m × 30 m;
- scaled working document;
- profile-defined lower effective PPI than close-view fixture;
- vector logo/text;
- raster photo.

Verify calculations without constructing an unnecessary billions-of-pixels flattened bitmap.

### Fixture C — low-PPI failure
Image placed so effective PPI falls below profile minimum.

### Fixture D — profile-version change
Preflight on v1, profile changed to v2, then ensure prior certification cannot silently cover v2.

### Fixture E — disconnect
Adapter becomes unavailable mid-operation.

## 4. Output-package assertions

For print package:
- required filenames exist;
- checksums present;
- source exists;
- preview exists;
- job ticket identifies final size + unit + scale + profile version;
- preflight report matches revision;
- no unresolved blocker;
- export format satisfies profile.

## 5. Manual visual validation

Automated image similarity is not sufficient for design quality.

Real operator should inspect:
- hierarchy;
- spelling;
- logo correctness;
- copy;
- crop/safe areas;
- visual balance;
- expected scale/final size;
- print operator interpretation of output file.

## 6. CI gates

Per normal commit:
- format
- lint
- typecheck
- unit
- integration
- security-fast
- build

Per release candidate:
- full UI/E2E
- migration
- dependency/security audit
- package/installer
- real-host evidence
- UAT evidence

## 7. Evidence retention

For each milestone candidate:
- exact SHA;
- CI run;
- test summary;
- real-host version where applicable;
- screenshots/previews;
- sample artifacts or checksums;
- audit report.
