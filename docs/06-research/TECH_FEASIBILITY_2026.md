# Technical Feasibility Notes — September 2026

This file records foundation research used to avoid designing around nonexistent integration capabilities. Re-verify vendor documentation before implementation of each external adapter because vendor APIs change.

## OpenAI

OpenAI's current model catalog documents current frontier models available through the API and tool support including functions/computer use for applicable models. Vasin Astra should still use a provider abstraction and structured function/tool calls rather than bind product identity to a specific model ID.

Official source:
- https://platform.openai.com/docs/models

## Adobe Photoshop

Adobe UXP is the modern Photoshop plugin/scripting platform. Photoshop host APIs provide document/layer operations, and `batchPlay` can access Photoshop operations not exposed by higher-level DOM APIs.

Official sources:
- https://developer.adobe.com/uxp/
- https://developer.adobe.com/photoshop/uxp/2022/ps-reference/
- https://developer.adobe.com/uxp/guides/explanation/fundamentals/apis/

Architectural implication:
- a Photoshop in-host adapter is feasible;
- do not expose raw `batchPlay` directly to the LLM;
- the adapter should translate reviewed typed commands into DOM/batchPlay operations.

## Adobe Illustrator

Adobe provides Illustrator automation/scripting documentation and a Firefly Services Illustrator API. Current Illustrator API documentation includes custom scripts as a public-beta capability and workflows such as renditions/data merge/image trace.

Official sources:
- https://developer.adobe.com/firefly-services/docs/illustrator/
- https://developer.adobe.com/firefly-services/docs/illustrator/guides/custom-scripts/
- https://ai-scripting.docsforadobe.dev/

Architectural implication:
- keep Illustrator behind a replaceable adapter;
- verify the customer's installed Illustrator version and the exact local scripting mechanism before committing to one implementation;
- cloud Illustrator APIs may complement but should not silently replace a required local/offline workflow.

## Canva

Canva Connect APIs can create designs, sync assets, and export designs. A custom design created through the Connect API is constrained by Canva's API dimension/area limits. Canva also has an Apps SDK Design Editing API for programmatic editing inside Canva.

Official sources:
- https://www.canva.dev/docs/connect/
- https://www.canva.dev/docs/connect/api-reference/designs/create-design/
- https://www.canva.dev/docs/apps/design-editing/

Architectural implication:
- Canva is viable as an optional workflow/template integration;
- it should not be the only production path for extreme large-format physical designs;
- deeper element editing may require a Canva app context rather than only remote Connect API calls.

## Print resolution

Adobe documentation distinguishes image PPI from printer DPI and notes that 300 PPI is a common high-quality close-view target, while lower image resolution can be acceptable for large-format work viewed from a distance. Production requirements should come from the print service/printer profile.

Official sources:
- https://helpx.adobe.com/photoshop/desktop/crop-resize-transform/resize-adjust-resolution/printed-image-resolution.html
- https://helpx.adobe.com/photoshop/desktop/crop-resize-transform/resize-adjust-resolution/resolution-specs-for-printing-images.html

Architectural implication:
- never hard-code one PPI for all products;
- calculate effective final-size PPI;
- large-format should be vector-first and profile-driven.

## Bleed and PDF output

Adobe Illustrator documents printer marks/bleed and PDF output options. Actual bleed values depend on production workflow and should be configured per printer/profile.

Official sources:
- https://helpx.adobe.com/illustrator/using/printers-marks-bleeds.html
- https://helpx.adobe.com/illustrator/using/pdf-options.html

## Foundation conclusion

The product is technically feasible, but the correct architecture is not "one desktop app directly controlling every design app with arbitrary AI commands." The viable architecture is:

**AI planner → validated design/print specs → safe typed adapter commands → host application → deterministic preflight → export package.**
