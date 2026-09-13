# AGENTS.md — Vasin Astra Engineering Contract

## Purpose

This file governs all coding agents working in this repository.

Vasin Astra is a Windows desktop AI graphic-design and print-production system. It is not a generic image generator. Its primary obligation is to produce editable, production-aware design work and print-ready output for real printing jobs.

## Required lifecycle

UNDERSTAND → DOCUMENT → CONTRACT → REPOSITORY → BUILD → SELF-AUDIT → INDEPENDENT AUDIT → REMEDIATE → UAT → RELEASE

Do not skip phases because coding is possible.

## Source-of-truth precedence

When sources conflict, use this order:

1. `docs/04-delivery/ACCEPTANCE_CONTRACT.md`
2. `docs/02-domain/BUSINESS_RULES.md` and `docs/02-domain/PRINT_DOMAIN.md`
3. `docs/01-product/PRODUCT_REQUIREMENTS.md`
4. `docs/03-architecture/*`
5. `docs/04-delivery/IMPLEMENTATION_PLAN.md`
6. Existing implementation

Unknown or contradictory requirements go to `docs/00-governance/OPEN_ITEMS.md`; do not invent official print-shop rules.

## Hard engineering rules

- Build one milestone at a time.
- Never place API keys or OAuth secrets in source, renderer state, plaintext config, logs, test fixtures, or commits.
- Never execute raw shell, PowerShell, JSX, VBScript, UXP, or arbitrary code directly from model output.
- LLM output must be converted to validated typed commands and executed only through allow-listed adapters.
- Bind local integration bridges to loopback only unless an approved architecture decision says otherwise.
- Preserve editability: text, logos, shapes, and layout geometry should remain vector/editable whenever the target tool permits.
- Raster imagery must be evaluated at effective final-size PPI, not only source pixel dimensions.
- Do not hard-code 300 PPI for every print product. Printer profiles define target effective PPI and production constraints.
- Never silently upscale low-resolution assets and call them print-ready.
- Print dimensions must be stored in physical units and represented independently from document scale.
- Large-format scaling must be explicit and reversible; a 1:10 document must still carry a 1:1 final-size specification.
- Export cannot be marked "print-ready" without a completed preflight.
- Any manual preflight override must be explicit and auditable.
- Keep local files inside the configured workspace unless the user explicitly selects another destination.
- Destructive overwrite/delete operations require explicit confirmation.

## Required quality gates per milestone

As applicable:
- format
- lint
- typecheck
- unit tests
- integration tests
- API/adapter tests
- UI tests
- security tests
- migration tests
- real workflow tests
- build/package test

A green CI run is evidence, not proof by itself. The builder must provide exact-SHA evidence and a self-audit against the milestone contract.

## Handoff format

Every build handoff must include:
- milestone ID and title
- exact branch and commit SHA
- files changed
- acceptance criteria status
- commands/tests run and results
- screenshots/artifacts where applicable
- known limitations
- unresolved open items
- explicit statement that no later milestone was started

Do not merge unless instructed by the project owner.
