# Vasin Astra

**Vasin Astra** is a Windows desktop AI graphic-design and print-production system for Vasin, a professional printing agency.

The user works with an AI agent named **Astra** through conversation. Astra turns a print brief into a structured design plan, operates connected creative tools through safe adapters, supports revisions, performs deterministic print preflight, and produces editable source files plus validated print packages.

## Why this is different from a normal image generator

A print-production agent must understand more than visual appearance:

- final physical dimensions;
- document scaling for very large formats;
- effective raster PPI at final print size;
- vector/editable typography and logos;
- printer profiles;
- bleed and safe zones;
- color/output requirements;
- real Photoshop/Illustrator host state;
- preflight and production evidence.

A beautiful flattened image is not automatically a print-ready design.

## Engineering lifecycle

This repository follows the App Maker lifecycle:

**UNDERSTAND → DOCUMENT → CONTRACT → REPOSITORY → BUILD → SELF-AUDIT → INDEPENDENT AUDIT → REMEDIATE → UAT → RELEASE**

Unknown production rules are tracked in `docs/00-governance/OPEN_ITEMS.md`; they are not guessed.

## Documentation

Start with:
- `docs/INDEX.md`
- `docs/00-governance/PROJECT_CHARTER.md`
- `docs/01-product/PRODUCT_REQUIREMENTS.md`
- `docs/02-domain/PRINT_DOMAIN.md`
- `docs/03-architecture/SYSTEM_ARCHITECTURE.md`
- `docs/04-delivery/ACCEPTANCE_CONTRACT.md`
- `docs/04-delivery/IMPLEMENTATION_PLAN.md`

## Build and audit prompts

- Builder: `prompts/BUILD_MASTER_PROMPT.md`
- Independent auditor: `prompts/AUDIT_MASTER_PROMPT.md`

The builder and auditor use the **same acceptance contract**.

## Current state

Foundation documentation is prepared on the `docs/app-maker-foundation` branch. Application implementation has not started. See `docs/00-governance/PROJECT_STATUS.md`.
