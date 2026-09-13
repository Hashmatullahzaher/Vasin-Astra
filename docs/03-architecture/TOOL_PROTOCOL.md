# Tool Protocol

## 1. Goal

Provide a stable, safe boundary between Astra and external design hosts.

## 2. Envelope

Every command should use a versioned envelope:

```ts
type ToolCommand<T> = {
  protocolVersion: "1"
  operationId: string
  jobId: string
  revisionId: string
  tool: "PHOTOSHOP" | "ILLUSTRATOR" | "CANVA"
  command: string
  payload: T
  expectedDocumentId?: string
}
```

Result:

```ts
type ToolResult<T> = {
  protocolVersion: "1"
  operationId: string
  status: "SUCCEEDED" | "FAILED" | "PARTIAL" | "REJECTED"
  data?: T
  error?: {
    code: string
    message: string
    retryable: boolean
  }
  changedObjectRefs?: string[]
  hostDocumentId?: string
}
```

## 3. Initial command families

### Connection/document
- GET_CAPABILITIES
- CREATE_DOCUMENT
- OPEN_DOCUMENT
- INSPECT_DOCUMENT
- SAVE_DOCUMENT
- CLOSE_DOCUMENT

### Elements
- CREATE_TEXT
- UPDATE_TEXT
- CREATE_SHAPE
- PLACE_ASSET
- REPLACE_ASSET
- TRANSFORM_ELEMENT
- SET_ELEMENT_STYLE
- GROUP_ELEMENTS
- SET_VISIBILITY
- DELETE_ELEMENT

### Production
- SET_COLOR_MODE where supported
- SET_BLEED_METADATA where supported
- RENDER_PREVIEW
- EXPORT_ARTIFACT

Host-specific special capabilities must remain explicit named commands.

## 4. Validation

- discriminated union by `command`;
- strict schemas;
- reject unknown fields where practical;
- unit/coordinate bounds;
- max string/payload size;
- safe file refs use Asset IDs or workspace-relative paths, never untrusted arbitrary OS paths.

## 5. Idempotency

Commands that create/modify must use operationId. Adapter should persist/reconcile recently completed operation IDs where feasible so retry cannot create duplicate documents/elements.

## 6. Capability negotiation

`GET_CAPABILITIES` returns:
- adapter version;
- protocol versions;
- host app/version;
- command list;
- export formats;
- size/document limits known to adapter;
- feature flags.

Agent planner must not emit unsupported commands.

## 7. No escape hatch

Production protocol intentionally excludes:
- RUN_CODE
- RUN_SHELL
- RUN_SCRIPT
- RAW_BATCH_PLAY
- EVAL
- arbitrary HTTP

If a missing operation is legitimate, implement a reviewed named command with tests.
