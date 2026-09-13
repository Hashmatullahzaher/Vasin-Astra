# Glossary

| Term | Meaning |
|---|---|
| Astra | The AI design/production agent inside Vasin Astra. Astra is a product agent identity, not a hard-coded model ID. |
| Final size | Physical dimensions of the printed product at 1:1, independent of design-document scale. |
| Document scale | Ratio between the editable document and final physical output, e.g. 1:1, 1:2, 1:5, 1:10. |
| Effective PPI | Raster pixels per inch at final physical output size after scaling. |
| Source PPI | Raster resolution relative to the working document. |
| Print profile | Stored production rules for a print process/product/printer, including target effective PPI, color expectations, bleed, scale policy, outputs, and tolerances. |
| Preflight | Automated validation before an artifact can be called print-ready. |
| Hard fail | A preflight problem that blocks print-ready export unless an authorized explicit override exists. |
| Warning | A preflight concern that may be accepted by a human reviewer. |
| Bleed | Artwork extending outside the trim/final boundary to tolerate cutting/finishing variance. |
| Safe zone | Interior margin in which critical text/logos should remain. |
| Trim/final boundary | Intended physical finished edge of the product. |
| Vector-first | Principle of preserving typography, logos, shapes, and line art as vectors/editable objects where possible. |
| Raster asset | Pixel-based image such as photo, generated background, JPEG, PNG, TIFF, or flattened artwork. |
| Design intent | Structured description of what should be made before tool-specific execution. |
| Design spec | Validated machine-readable design plan containing dimensions, print constraints, elements, assets, typography, layout, and output intent. |
| Tool adapter | Controlled interface between Astra and a design host such as Photoshop, Illustrator, or Canva. |
| Local bridge | Loopback-only IPC/HTTP/WebSocket mechanism between the desktop app and local design-tool plugin/adapter. |
| Reference design | Existing customer/agency work used for style/layout retrieval or comparison. |
| Editable source | Native project file intended for further human editing, such as PSD/PSB/AI depending workflow. |
| Print package | Final outputs, source file, preview, preflight report, and production metadata for a job. |
| Exact-SHA audit | Audit tied to one immutable Git commit SHA, ensuring the reviewed code is exactly the code claimed. |
