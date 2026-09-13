# Integrations

## 1. Integration philosophy

Creative applications are accessed through adapters. Core product logic never calls vendor APIs directly.

Common adapter contract:

```ts
interface DesignToolAdapter {
  connect(): Promise<ConnectionInfo>
  getCapabilities(): Promise<CapabilitySet>
  createDocument(input: CreateDocumentCommand): Promise<ToolResult>
  openDocument(input: OpenDocumentCommand): Promise<ToolResult>
  applyOperations(input: ApplyOperationsCommand): Promise<ToolResult>
  inspectDocument(input: InspectDocumentCommand): Promise<DocumentInspection>
  renderPreview(input: RenderPreviewCommand): Promise<PreviewResult>
  saveSource(input: SaveSourceCommand): Promise<SaveResult>
  exportArtifact(input: ExportCommand): Promise<ExportResult>
  health(): Promise<HealthResult>
}
```

Exact command types belong in `packages/tool-protocol`.

## 2. OpenAI

### Purpose
- Astra reasoning/planning;
- vision inspection of references/previews;
- structured design-spec generation;
- tool selection/function calling;
- optional image generation through a separate image provider.

### Rules
- model ID is configuration;
- provider calls occur in privileged service layer;
- API key stored by CredentialRef;
- structured outputs validated;
- retries bounded;
- tool permissions enforced outside model;
- cost/token metadata recorded when available.

### Initial strategy
Use a capable OpenAI model through the current Responses API-compatible SDK surface. Use cheaper/faster model tiers for low-risk classification/summarization only after quality evaluation.

## 3. Photoshop

### Preferred pattern
A dedicated Photoshop in-host plugin/adapter using Adobe UXP/Photoshop APIs and a secure loopback channel to Vasin Astra.

Capabilities targeted:
- host/version discovery;
- create/open document;
- layers/groups;
- text layer operations;
- shape/vector-like operations where supported;
- place/import assets;
- transforms;
- masks/adjustments where required;
- smart objects where appropriate;
- document inspection;
- preview export;
- source save;
- export.

For Photoshop operations not covered by high-level DOM APIs, Adobe's `batchPlay` may be used inside the vetted adapter implementation.

### Security
The plugin exposes named operations, not a generic raw `batchPlay`/code relay from the LLM.

## 4. Illustrator

### Adapter requirement
Keep Illustrator behind `IllustratorAdapter`.

Potential verified implementations:
1. local Illustrator scripting/automation using vetted scripts and parameter payloads;
2. Adobe Illustrator API for supported/cloud use cases;
3. hybrid.

The production choice must be proven against the customer's actual Illustrator version and licensing.

### Do not
- expose a raw "run arbitrary JSX" LLM tool;
- assume a UXP implementation without verified host support;
- depend on a cloud-only feature for a workflow that must remain local unless approved.

## 5. Canva

Canva has two different integration surfaces relevant to this product:

### Canva Connect APIs
Useful from Vasin Astra for:
- OAuth account connection;
- asset sync;
- design creation;
- design metadata;
- export;
- brand-template/autofill workflows where plan/API requirements permit.

### Canva Apps SDK
Runs an app inside Canva and can programmatically interact with design elements through Canva's design APIs.

### Architectural decision
Treat Canva as an optional adapter and do not rely on Canva as the final production host for extreme physical formats. A Canva-created concept can still be imported into a production workflow if required.

## 6. Image generation

Define:

```ts
interface ImageGenerationProvider {
  generate(request: ImageGenerationRequest): Promise<GeneratedImage>
  edit?(request: ImageEditRequest): Promise<GeneratedImage>
}
```

Every result must record:
- provider/model;
- generated dimensions;
- source prompt summary/hash as policy allows;
- job/revision;
- file checksum.

It enters the job as a raster Asset and must pass normal PPI/preflight rules.

## 7. Future integrations

Possible:
- Adobe Firefly services;
- cloud storage;
- fonts/licensing services;
- RIP/printer hot folders;
- QR/barcode generation;
- customer CRM/order system.

Each requires a separate acceptance contract before becoming production-critical.
