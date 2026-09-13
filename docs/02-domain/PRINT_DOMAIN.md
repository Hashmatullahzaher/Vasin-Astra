# Print Domain Model

## 1. Core principle

The print product is defined in **physical output space**. Creative applications are execution hosts, not the source of truth for final physical intent.

Store these separately:

- final width/height;
- physical unit;
- document scale;
- working document width/height;
- bleed;
- safe zone;
- target effective PPI;
- color/output requirements.

## 2. Dimension model

Let:
- `F` = final physical dimension;
- `S` = scale denominator (1 for 1:1, 10 for 1:10);
- `W = F / S` = working physical dimension.

Example:
- final width = 30 m;
- scale = 1:10;
- working width = 3 m.

The user interface must show both:
- **Final: 2 m × 30 m**
- **Working document: 200 mm × 3000 mm @ 1:10** (example only)

Never infer final size solely from host document size when scale != 1.

## 3. Raster effective PPI

Effective final PPI is determined by source pixels and final placed physical size.

For one dimension:

`effective_ppi = raster_pixels / final_placed_inches`

When a working document is scaled, a raster's working-document resolution must be interpreted through that scale. Preflight should calculate from source pixels and final placed size whenever possible because it is less error-prone.

## 4. Resolution policy

Do not use a universal print-resolution constant.

A PrinterProfile defines:
- minimum effective PPI (hard fail threshold);
- recommended effective PPI;
- optional maximum/useful PPI;
- viewing-distance assumptions;
- exception policy.

Close-view print commonly uses higher resolution. Large-format work may legitimately use lower effective PPI when viewed from a distance. The production profile is authoritative.

## 5. Vector-first policy

Prefer vectors/editable primitives for:
- text;
- logos;
- QR/barcodes;
- shapes;
- lines;
- icons;
- cut/contour lines where supported.

Raster imagery is appropriate for:
- photographs;
- textures;
- generated visual backgrounds;
- raster artwork intentionally supplied by the customer.

Do not rasterize the full design only to simplify tool automation.

## 6. Bleed and safe zones

Bleed and safe zone are PrinterProfile values or per-job approved overrides.

Preflight checks:
- background/edge artwork extends through required bleed;
- critical elements stay inside safe zone;
- trim/final bounds are known.

No global bleed value is assumed because production methods differ.

## 7. Color

PrinterProfile may define:
- RGB allowed/prohibited;
- required CMYK/ICC profile;
- spot colors;
- black construction rules;
- total ink limit metadata if relevant;
- rendering intent/export preset.

The application should detect what is technically available from the host. If a required color condition cannot be verified automatically, preflight reports MANUAL_CHECK rather than claiming success.

## 8. Typography

Track:
- font family/style;
- font availability;
- embedding/outline policy from profile;
- minimum text-size policy if configured.

Never silently substitute a missing font in a final print job.

## 9. Output formats

Each PrinterProfile can define required/allowed outputs, e.g.:
- PDF/PDF-X preset;
- AI;
- PSD/PSB;
- TIFF;
- JPEG;
- SVG for certain workflows;
- preview PNG/JPEG.

Do not label an output "print-ready" solely because its extension is accepted.

## 10. Product types

Initial master data may include:
- business card;
- flyer;
- brochure;
- poster;
- indoor banner;
- outdoor banner;
- billboard/sign;
- sticker/label;
- roll-up;
- menu;
- invitation;
- letterhead;
- envelope;
- custom print product.

These are templates/categories, not hard-coded production truth. Profiles carry actual constraints.

## 11. Preflight severity

### BLOCKER
Violates a non-overridable or not-yet-overridden production requirement.

### WARNING
Potential quality/production concern accepted by profile policy or requiring human review.

### INFO
Useful production metadata.

### MANUAL_CHECK
System lacks reliable machine evidence to certify the condition.

## 12. Print-ready definition

A design is print-ready only when:
- a PrinterProfile is selected;
- final dimensions are explicit;
- required preflight checks ran;
- no unresolved blockers remain;
- required manual checks are acknowledged where policy allows;
- export completed successfully;
- output metadata and preflight report were persisted.
