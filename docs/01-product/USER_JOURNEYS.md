# User Journeys

## Journey A — Business card

1. Operator selects **New Job → Business Card**.
2. Enters final size (example test profile may use 90 × 50 mm; real Vasin defaults are configuration, not hard-coded truth).
3. Selects a printer profile.
4. Adds logo, name, title, phone, address, QR code, colors.
5. Astra creates a structured design plan.
6. Vector-oriented host is selected where appropriate.
7. Astra builds front/back artboards while preserving text/logo editability.
8. Preview is shown.
9. Operator says: "Make the logo smaller and move the phone number to the bottom."
10. Astra modifies the existing document.
11. Preflight validates final size, bleed, safe zone, fonts, raster assets, and export requirements.
12. Print package is exported with editable source and preflight report.

## Journey B — 2 m × 30 m outdoor banner

1. Operator enters **2 m × 30 m** as final size.
2. Selects Vasin's outdoor-banner printer profile.
3. System does **not** blindly create a 300-PPI 1:1 raster canvas.
4. Print Engine uses the profile to determine/validate:
   - working scale (for example 1:10 only if profile permits);
   - target effective final-size PPI;
   - bleed/safe zone;
   - color/output rules.
5. Operator supplies logo, copy, and photos.
6. Astra keeps logo/text/shapes vector where possible.
7. Any raster photo is checked at effective final-size PPI.
8. If a photo is insufficient, Astra reports the exact production problem and proposes permitted actions (replace, approved upscale workflow, reduce placed size, or accept authorized warning if profile permits).
9. Astra builds the design and generates previews.
10. After revision, preflight confirms dimensions and profile rules.
11. Export package includes final-size metadata so a scaled working document cannot be misinterpreted as final size.

## Journey C — Revision by conversation

1. Operator opens a job revision.
2. Says: "Use the previous version, keep the background, replace the product image, make the headline more visible, and do not change the logo."
3. Astra resolves target elements from the design spec/host document.
4. Astra sends only bounded edit commands.
5. Existing design is revised in place.
6. A new Revision record and preview are created.

## Journey D — Failed preflight

1. Operator requests final export.
2. Preflight detects a raster image below required effective PPI and missing font.
3. Export state is BLOCKED.
4. Astra explains both issues.
5. Operator fixes them or, if authorized and the profile permits override, records an override reason.
6. Final package clearly records any accepted warnings/overrides.

## Journey E — Tool unavailable

1. Operator asks Astra to modify a Photoshop design.
2. Photoshop adapter is disconnected or incompatible.
3. Astra must not pretend the edit happened.
4. UI shows connection failure and recovery steps.
5. Job and design plan remain intact; operator can reconnect and retry.
