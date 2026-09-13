# Project Charter — Vasin Astra

## 1. Project

**Name:** Vasin Astra  
**Customer:** Vasin, a professional printing agency  
**Product type:** Installable Windows desktop application  
**Primary concept:** An AI graphic-design and print-production agent named **Astra** that can converse with a printing-agency operator, plan a design, use connected design tools, revise work, preflight it, and produce editable and print-ready deliverables.

## 2. Business problem

Printing agencies repeatedly create business cards, banners, brochures, posters, flyers, signs, and other print products. Conventional design workflows require a skilled operator to translate a customer brief into dimensions, artwork, typography, images, color settings, bleed/safe zones, production files, and final exports.

Generic AI image generators are insufficient because:
- output is often a flattened raster;
- text and logos may not remain editable;
- physical print size is not treated as a first-class requirement;
- huge formats such as 2 m × 30 m require scale-aware production;
- effective resolution, color, bleed, fonts, links, and export constraints must be checked before printing;
- the printing agency needs files that can actually move into its production workflow.

Vasin Astra must bridge conversational AI with professional design software and a deterministic print-production engine.

## 3. Product objective

Enable an operator to say, for example:

> "Create a 2 m × 30 m outdoor banner for this customer using this logo, this photo, these words, and our printer profile."

Astra should:
1. extract/confirm the job brief;
2. create a structured design intent;
3. determine the correct document scale and production constraints;
4. generate or source required imagery;
5. build/edit the design through an appropriate connected design tool;
6. render previews for review;
7. apply requested revisions;
8. run print preflight;
9. export an editable source plus requested print-ready outputs;
10. preserve an audit trail of what it did.

## 4. Initial product strategy

### V1 priority
A local-first Windows application focused on reliable print workflows, with:
- OpenAI-backed Astra agent;
- print job/project management;
- printer profiles;
- deterministic print calculations and preflight;
- Photoshop integration;
- Illustrator integration;
- preview/revision loop;
- export package;
- secure credentials;
- installer and UAT.

### Later/optional
- Canva integration;
- reference-design learning;
- brand-memory retrieval;
- voice conversation;
- cloud collaboration;
- multi-tenant SaaS control plane;
- mobile companion;
- automated pricing/quotation;
- RIP/printer direct integration.

## 5. Success criteria

V1 is successful when a real Vasin operator can complete at least:
- one close-view small-format job (business card class);
- one large-format job (multi-meter banner class);

from brief to editable source and validated print package without manually reconstructing the design.

## 6. Non-goals for initial release

- Replacing Adobe licensing.
- Building a full Photoshop/Illustrator clone.
- Guaranteeing that any arbitrary generated image is suitable for any final print size.
- Directly driving production printers/RIPs without a separately approved integration.
- Public customer self-service portal.
- Unrestricted autonomous OS control.
- Unrestricted execution of AI-generated code.

## 7. Delivery method

The project uses the App Maker engineering lifecycle:

UNDERSTAND → DOCUMENT → CONTRACT → REPOSITORY → BUILD → SELF-AUDIT → INDEPENDENT AUDIT → REMEDIATE → UAT → RELEASE

Documentation and acceptance criteria precede implementation. Unknown official rules are tracked, not guessed.
