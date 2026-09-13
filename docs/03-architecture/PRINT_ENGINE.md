# Print Engine and Preflight Design

## 1. Purpose

The Print Engine is the deterministic production core. It converts human-friendly print requirements into validated production parameters and verifies outputs before certification.

It must be usable independently from the LLM.

## 2. Modules

```text
print-engine/
  units
  dimensions
  scale
  raster-resolution
  bleed-safe-zone
  profile
  preflight
  output-intent
  calculators
```

## 3. Unit handling

Canonical unit: millimeter.

Conversions must be tested for:
- mm ↔ cm
- mm ↔ m
- mm ↔ inch

Avoid lossy equality comparisons. Use decimal-safe values or explicit tolerances.

## 4. Working scale

A PrinterProfile can define allowed scales, e.g. 1:1, 1:2, 1:5, 1:10.

Algorithm:
1. take final physical dimensions;
2. inspect target host/document limits and profile policy;
3. select or request approved scale;
4. compute working dimensions;
5. persist final + working dimensions;
6. never discard scale metadata.

Scale selection may be suggested by Astra but validated by the engine.

## 5. Raster PPI calculator

For an Asset placed at final size:
- final_width_inches = final_placed_width_mm / 25.4
- final_height_inches = final_placed_height_mm / 25.4
- ppi_x = source_width_px / final_width_inches
- ppi_y = source_height_px / final_height_inches
- effective_ppi = min(ppi_x, ppi_y) for conservative threshold checking

Account for crop: use the pixels actually used where this can be determined.

## 6. Example large-format implication

A multi-meter banner at 1:1 and close-view 300 PPI can imply billions of pixels and huge memory/storage. Therefore:
- text/logo/layout should remain vector;
- photographs should be placed assets;
- target effective PPI comes from the actual production profile/viewing distance;
- scaled working documents are first-class;
- preflight evaluates final output, not merely working-document PPI.

## 7. Preflight pipeline

Input:
- Job
- PrintSpec
- pinned PrinterProfileVersion
- DesignDocument inspection
- Asset metadata
- requested output format

Checks return:

```ts
type Finding = {
  code: string
  severity: "BLOCKER" | "WARNING" | "INFO" | "MANUAL_CHECK"
  message: string
  evidence?: Record<string, unknown>
  elementRef?: string
  overridable: boolean
}
```

## 8. Minimum checks

### Dimensions
- final width/height present;
- host working dimensions correspond to stored scale within tolerance.

### Scale
- scale allowed by profile;
- exported/job-ticket final size explicitly stated.

### Bleed
- configured against profile;
- content coverage check where host inspection supports it.

### Safe zone
- critical elements inside safe region where element geometry is available.

### Raster effective PPI
- every production raster placement measured;
- below minimum => BLOCKER unless profile says otherwise;
- between minimum/recommended => WARNING as profile defines.

### Assets
- no missing/unresolved links;
- checksums/path available where relevant.

### Fonts
- missing font => BLOCKER or MANUAL_CHECK according to profile/export method.

### Color
- verify document mode/profile when adapter can;
- otherwise MANUAL_CHECK;
- detect forbidden RGB/CMYK state where technically reliable.

### Output format
- requested export is allowed/required by profile.

### Source
- editable source saved and accessible if required.

## 9. Preflight result state

```text
Any unresolved BLOCKER -> BLOCKED
No blocker + warning/manual check -> PASS_WITH_WARNINGS
No blocker/warning requiring attention -> PASS
Engine error -> ERROR
```

## 10. Override

Override is not deletion of a finding.

Store:
- finding ID;
- actor;
- reason;
- timestamp;
- policy permitting override.

Preflight report shows:
- original severity;
- override state;
- reason.

## 11. Export gate

`canExportAsPrintReady(job)` requires:
- latest relevant revision;
- completed preflight tied to that revision;
- pinned profile version;
- no unresolved blockers;
- required approvals;
- valid destination.

A normal preview export may remain available even if print-ready export is blocked.

## 12. Test vectors

The test suite must include:
- mm/inch conversion;
- 1:1 and 1:10 scale;
- portrait/landscape;
- very large dimensions;
- exact threshold PPI;
- just below/above threshold;
- crop/placement cases;
- missing profile;
- profile version change after preflight;
- override authorization;
- host dimension mismatch;
- integer overflow/very large pixel counts.

Property-based tests are encouraged for unit/scale invariants.
