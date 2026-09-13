# Open Items

Unknowns are tracked here instead of being invented. An unresolved item blocks only the feature that depends on it.

## P0 — decisions needed before the dependent milestone is accepted

### OI-001 — Customer's exact production environment
- Which Windows versions must be supported?
- Exact Photoshop version(s)?
- Exact Illustrator version(s)?
- Does every target workstation have both applications installed?
- Status: OPEN
- Blocks: final compatibility matrix for Photoshop/Illustrator adapters.

### OI-002 — Printer/RIP production profiles
For each main product class, obtain Vasin's real production rules:
- printer/RIP name;
- substrate/material;
- required final file format;
- target effective PPI or accepted range;
- color space/profile/ICC expectations;
- document-scale convention for large format;
- bleed and safe-margin rules;
- crop/registration marks;
- font policy;
- transparency/rasterization policy;
- spot-color/cut-contour conventions;
- maximum file/document dimensions.
- Status: OPEN
- Blocks: production certification. Does **not** block building the profile engine.

### OI-003 — AI provider/account model
Decide whether V1 uses:
- a Vasin-owned OpenAI API key;
- a customer-supplied OpenAI API key;
- both, selectable by admin.
- Status: OPEN
- Blocks: final onboarding and billing UX.

### OI-004 — Illustrator integration mechanism
Choose the verified production route after testing Vasin's Illustrator version:
- local vetted Illustrator scripting bridge (JSX/COM/VBScript on Windows);
- Adobe Illustrator API where licensing/access fits;
- a combination.
Do not assume an Illustrator UXP path without compatibility proof.
- Status: OPEN
- Blocks: final Illustrator adapter implementation.

### OI-005 — Canva scope
Is Canva required for V1, or acceptable as Phase 2?
Current architecture treats Canva as optional because high-end print production remains centered on Adobe/native source workflows.
- Status: OPEN
- Blocks: Canva milestone priority only.

### OI-006 — Required languages
Confirm application UI and Astra conversation languages:
- English only;
- Dari/Farsi + English;
- additional languages.
- Status: OPEN
- Blocks: localization acceptance.

### OI-007 — Internet/offline behavior
Define which workflows must operate without internet. AI/image generation and cloud APIs generally require connectivity; local project/history/preflight can be designed to remain available.
- Status: OPEN
- Blocks: offline acceptance requirements.

### OI-008 — Cloud accounts and licensing
Confirm whether V1 is:
- one local workstation;
- several Vasin workstations sharing jobs;
- licensed per workstation/user.
- Status: OPEN
- Blocks: final auth, sync, licensing, and cloud-control-plane scope.

## P1 — can be resolved during implementation

### OI-009 — Preferred editable master format by product
Examples: AI for vector-heavy large format, PSD/PSB for photo-compositing jobs.
- Status: OPEN

### OI-010 — Default export bundle
Confirm which of PDF/X, TIFF, JPEG, SVG, PNG preview, AI, PSD/PSB are routinely required for each product.
- Status: OPEN

### OI-011 — Existing Vasin templates/reference designs
Need a representative sample set if style learning/retrieval is included.
- Status: OPEN

### OI-012 — Human approval policy
Decide which operations require explicit confirmation:
- first execution of a design plan;
- destructive overwrite;
- preflight override;
- final print-ready export;
- external upload.
- Status: OPEN

### OI-013 — Auto-updates and telemetry
Decide whether V1 should auto-update and whether diagnostics/telemetry may leave the device.
- Status: OPEN
