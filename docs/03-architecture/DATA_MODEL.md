# Data Model

## 1. Persistence strategy

V1 uses SQLite for structured local state and the filesystem for native design documents/assets/exports.

Store IDs and metadata in the database; do not place large native files inside SQLite.

## 2. Core entities

### Workspace
- id
- name
- root_path
- created_at
- updated_at

### User
- id
- display_name
- role
- local_identity_ref
- created_at
- updated_at

### Job
- id
- workspace_id
- customer_name
- title
- product_type_id
- status
- current_revision_id
- printer_profile_version_id
- created_by
- created_at
- updated_at
- archived_at

Suggested statuses:
- DRAFT
- BRIEF_READY
- PLANNED
- IN_DESIGN
- REVIEW
- PREFLIGHT_BLOCKED
- READY_TO_EXPORT
- EXPORTED
- APPROVED
- ARCHIVED

### JobBrief
- id
- job_id
- copy_json
- style_direction
- quantity
- substrate
- viewing_distance
- finishing_notes
- deadline
- freeform_notes
- created_at
- updated_at

### PrintSpec
- id
- job_id
- final_width_mm
- final_height_mm
- original_unit
- scale_numerator
- scale_denominator
- working_width_mm
- working_height_mm
- bleed_json_mm
- safe_margin_json_mm
- target_effective_ppi
- minimum_effective_ppi
- color_requirement_json
- output_requirement_json
- profile_version_id
- created_at
- updated_at

Use decimal-safe storage for physical measurements. Do not depend on binary floating-point equality for acceptance.

### PrinterProfile
- id
- stable_key
- name
- active_version_id

### PrinterProfileVersion
- id
- printer_profile_id
- version
- configuration_json
- status
- created_by
- created_at

Exports pin the exact profile version.

### Asset
- id
- job_id
- type
- origin
- original_name
- relative_path
- mime_type
- width_px
- height_px
- embedded_color_profile
- checksum
- metadata_json
- created_at

Types:
- LOGO
- PHOTO
- GENERATED_IMAGE
- ICON
- FONT_REFERENCE
- DOCUMENT
- OTHER

Origins:
- USER
- GENERATED
- IMPORTED_FROM_HOST
- REFERENCE_LIBRARY

### DesignDocument
- id
- job_id
- host_tool
- host_document_id
- relative_source_path
- format
- document_scale_json
- last_known_host_version
- last_saved_at
- checksum
- created_at
- updated_at

### Revision
- id
- job_id
- parent_revision_id
- design_document_id
- sequence
- user_instruction
- design_spec_json
- execution_summary
- preview_asset_id
- state
- created_by
- created_at

### AgentRun
- id
- job_id
- revision_id
- provider
- model
- purpose
- input_summary
- output_summary
- token_usage_json
- cost_metadata_json
- state
- started_at
- completed_at
- error_json

Do not store raw secrets or unnecessarily sensitive full prompts in logs.

### ToolCall
- id
- agent_run_id
- operation_id
- tool_type
- adapter_version
- command_type
- sanitized_payload_json
- status
- started_at
- completed_at
- structured_result_json
- error_json

### PreflightRun
- id
- job_id
- revision_id
- profile_version_id
- status
- started_at
- completed_at
- summary_json

Statuses:
- PASS
- PASS_WITH_WARNINGS
- BLOCKED
- ERROR

### PreflightFinding
- id
- preflight_run_id
- check_type
- severity
- code
- message
- evidence_json
- element_ref
- override_status
- override_reason
- overridden_by
- overridden_at

### ExportArtifact
- id
- job_id
- revision_id
- preflight_run_id
- format
- relative_path
- checksum
- final_width_mm
- final_height_mm
- document_scale_json
- profile_version_id
- created_at

### IntegrationConnection
- id
- tool_type
- connection_mode
- host_version
- adapter_version
- capability_json
- status
- last_seen_at
- config_json

No secret values in config_json.

### CredentialRef
- id
- provider
- os_secret_key
- label
- created_at
- updated_at

The actual secret lives in OS-backed secure storage.

### AuditEvent
- id
- job_id nullable
- actor_id
- event_type
- entity_type
- entity_id
- sanitized_details_json
- created_at

## 3. Optional/future entities

- BrandKit
- ReferenceDesign
- ReferenceEmbedding
- Customer
- SharedWorkspaceMembership
- Approval
- License
- CloudSyncCursor

Do not build these before their milestone unless required by accepted scope.

## 4. Integrity constraints

- Job current_revision_id must belong to the same Job.
- Revision parent must belong to the same Job.
- ExportArtifact revision/preflight/profile references must agree on Job.
- PrintSpec profile_version must equal the selected profile version used by accepted preflight.
- Relative file paths must not escape Workspace root.
- CredentialRef never contains secret plaintext.
- A print-ready/exported certification cannot reference a BLOCKED preflight.
- Override records must include actor + reason + time.
- Operation IDs should be unique for externally executed commands.

## 5. Migration rule

Every schema change after the first release requires a forward migration and, when practical, a tested rollback/recovery path. Never mutate production SQLite schemas ad hoc.
