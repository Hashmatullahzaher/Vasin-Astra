# Continuous Build Mode

This document defines how an autonomous builder may continue useful work without inventing requirements.

## Rule 1 — Unknown is not permission to guess

If a required official fact is missing:
1. create/reference an OPEN_ITEMS entry;
2. build stable interfaces/placeholders when useful;
3. continue independent work;
4. stop only the dependent acceptance criterion.

Example:
If Vasin's banner printer effective-PPI target is unknown, build the PrinterProfile field and calculation engine, but do not hard-code a guessed production target.

## Rule 2 — One milestone only

A builder may complete all tasks inside the active milestone but may not start the next milestone automatically.

## Rule 3 — Documentation-first change control

If implementation exposes a contradiction in accepted product/domain rules:
- do not quietly change behavior;
- report it;
- propose a documentation/contract change;
- wait for approval when it alters product truth.

## Rule 4 — Engineering freedom where product truth is unaffected

Builder may choose normal implementation details such as:
- module naming;
- internal refactors;
- test helper structure;
- minor UI component organization;

provided accepted architecture/security/behavior is preserved.

## Rule 5 — Self-audit is mandatory

Before handoff:
- compare every active milestone criterion;
- run all required tests;
- inspect git diff;
- confirm no secrets;
- confirm no out-of-scope milestone work;
- report exact SHA.

## Rule 6 — Independent audit is separate

The builder must not declare its own self-audit equivalent to independent acceptance.

## Rule 7 — Remediation is bounded

After independent audit:
- fix only accepted findings;
- do not opportunistically start new features;
- rerun affected + regression tests;
- provide new exact SHA;
- re-audit.

## Rule 8 — Preserve working components

Do not rewrite functioning code merely to match a stylistic preference or imagined framework ideal. Change working components only for accepted requirements, defects, security/integrity, or meaningful maintainability reasons.
