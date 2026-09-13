# Master Data

## ProductType
Fields:
- id
- name
- category
- default_orientation (optional)
- suggested_host_tool (optional)
- active

No production-critical numeric values are authoritative here unless explicitly approved.

## PrinterProfile
Fields:
- id
- name
- version
- product_type_ids
- printer_or_rip_name
- substrate
- allowed_scale_ratios
- preferred_scale_ratio
- min_effective_ppi
- recommended_effective_ppi
- color_space_requirement
- icc_profile_name
- bleed_top/right/bottom/left + unit
- safe_margin_top/right/bottom/left + unit
- allowed_output_formats
- required_output_formats
- pdf_preset_name / PDF-X requirement if relevant
- font_policy
- transparency_policy
- mark_policy
- raster_override_policy
- notes
- active
- created_at/updated_at

## OutputFormat
Examples:
- PDF
- PDF/X-4
- AI
- PSD
- PSB
- TIFF
- JPEG
- PNG preview
- SVG

Actual support is adapter/profile dependent.

## ToolType
- PHOTOSHOP
- ILLUSTRATOR
- CANVA
- OPENAI_IMAGE
- OTHER_FUTURE

## PreflightCheckType
Initial:
- FINAL_DIMENSIONS
- DOCUMENT_SCALE
- BLEED
- SAFE_ZONE
- RASTER_EFFECTIVE_PPI
- MISSING_ASSET
- FONT_AVAILABILITY
- COLOR_MODE
- COLOR_PROFILE
- SPOT_COLOR
- OUTPUT_FORMAT
- EDITABLE_SOURCE_SAVED
- LINKED_ASSET
- MANUAL_PRODUCTION_CHECK

## Unit
Canonical internal physical base should be millimeters, with conversion for:
- mm
- cm
- m
- inch

Persist original user unit for display/history while calculations use a canonical precise representation.
