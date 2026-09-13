# Astra AI Agent Architecture

## 1. Identity

**Astra** is the user-facing AI graphic-design and print-production agent.

Astra is not the name of a single vendor model. The agent is an orchestration layer that combines:
- an LLM provider;
- structured product context;
- design schemas;
- deterministic print logic;
- safe tool adapters;
- revision history;
- visual inspection.

## 2. Agent operating loop

```text
UNDERSTAND BRIEF
      ↓
RESOLVE PRODUCTION SPEC
      ↓
PLAN DESIGN (DesignSpec)
      ↓
VALIDATE SCHEMAS + PRINT RULES
      ↓
USER APPROVAL (policy dependent)
      ↓
EXECUTE TYPED TOOL COMMANDS
      ↓
RENDER PREVIEW + INSPECT
      ↓
REVISE / USER FEEDBACK
      ↓
PREFLIGHT
      ↓
EXPORT
```

## 3. Internal agent stages

### Stage A — Brief interpreter
Extract:
- target product;
- dimensions;
- text/copy;
- assets;
- style;
- print profile;
- missing critical information.

### Stage B — Production planner
Uses deterministic Print Engine to resolve:
- final physical size;
- document scale;
- working size;
- effective PPI requirements;
- bleed/safe zone;
- output constraints.

The model cannot override the engine.

### Stage C — Art director
Creates layout/style intent:
- hierarchy;
- composition;
- typography;
- visual language;
- palette;
- asset roles.

### Stage D — Design executor
Maps DesignSpec operations to adapter-supported typed commands.

### Stage E — Visual critic
Inspects preview against:
- brief;
- alignment/hierarchy;
- text visibility;
- obvious collisions;
- reference direction;
- production safe zones where overlaid.

Visual critique is advisory; preflight is deterministic.

### Stage F — Production preflight
Runs Print Engine checks. No LLM-only certification.

## 4. Structured DesignSpec

Conceptual schema:

```json
{
  "schemaVersion": "1.0",
  "jobId": "...",
  "revisionId": "...",
  "printSpecRef": "...",
  "targetTool": "ILLUSTRATOR",
  "canvas": {
    "workingWidthMm": 3000,
    "workingHeightMm": 200,
    "scale": "1:10"
  },
  "elements": [
    {
      "id": "headline",
      "type": "TEXT",
      "content": "...",
      "bounds": {"x":0,"y":0,"w":0,"h":0},
      "style": {},
      "lockedByInstruction": false
    }
  ],
  "outputIntent": {}
}
```

Real implementation must use versioned TypeScript schemas and validation.

## 5. Tool policy

Allowed pattern:
- `photoshop.createDocument({...})`
- `photoshop.setText({...})`
- `illustrator.placeAsset({...})`
- `design.renderPreview({...})`

Forbidden production pattern:
- `run_shell("whatever the model wrote")`
- `run_jsx(rawModelText)`
- `batchPlay(rawModelJSON)` directly from model
- `write_file(arbitraryAbsolutePath)`

If an operation is needed often, add a reviewed typed command.

## 6. Revision behavior

When revising:
- read latest DesignSpec + host inspection;
- identify requested deltas;
- preserve locked/unchanged elements;
- apply minimal patch;
- create child Revision;
- generate new preview.

Do not regenerate entire artwork unless requested or required by host limitations.

## 7. Context strategy

Agent context may include:
- current brief;
- PrintSpec;
- PrinterProfile summary;
- current DesignSpec;
- previous instruction/revision summary;
- thumbnails/previews;
- selected reference-design summaries;
- adapter capabilities.

Do not inject the entire project history if a compact state is sufficient.

## 8. Reference learning — later phase

Reference designs can be indexed by:
- product type;
- color palette;
- typography;
- layout characteristics;
- brand/customer;
- semantic description;
- preview embedding.

Retrieval should inspire/guide Astra but not copy copyrighted third-party artwork or override explicit job requirements.

## 9. Hallucination controls

Astra must never state:
- "Photoshop saved the file" without adapter confirmation;
- "300 PPI" as universal truth;
- "print-ready" without passed/approved preflight;
- "font embedded" unless verified;
- "CMYK profile correct" unless verified;
- a printer-specific requirement not present in the selected profile.

Use terms such as MANUAL_CHECK when technical evidence is unavailable.

## 10. Loop limits

Autonomous visual-revision loops need bounded attempts, e.g. configurable maximum iterations. On repeated failure, stop and ask the operator instead of consuming unlimited tokens or making destructive repeated edits.
