# User Roles

## V1 roles

### Owner / Admin
Can:
- configure integrations and secrets;
- manage printer profiles;
- manage workspace/application settings;
- create/edit/export jobs;
- approve preflight overrides;
- view audit history.

### Designer / Operator
Can:
- create/edit jobs;
- chat with Astra;
- attach assets;
- run design operations;
- request revisions;
- run preflight;
- export when no blocking issue exists.

Preflight override permission is configurable and is **off by default** for this role.

### Print Reviewer
Optional role for multi-user deployments. Can:
- inspect final preview/spec;
- run preflight;
- approve/reject a print package;
- add production notes.

Cannot manage API credentials unless also an Admin.

## V1 deployment simplification

If V1 is deployed to a single operator workstation, the local user may initially act as Owner/Admin + Designer/Operator. The domain model should still retain explicit permissions so a later multi-user control plane does not require rewriting critical authorization logic.
