# Roles and Permissions

| Capability | Owner/Admin | Designer/Operator | Print Reviewer |
|---|---:|---:|---:|
| View jobs | ✓ | ✓ | ✓ |
| Create/edit jobs | ✓ | ✓ | review notes only |
| Run Astra design actions | ✓ | ✓ | optional/read-only by policy |
| Manage printer profiles | ✓ |  |  |
| Manage AI/API credentials | ✓ |  |  |
| Configure design-tool adapters | ✓ |  |  |
| Run preflight | ✓ | ✓ | ✓ |
| Override blocking preflight | ✓ | policy-controlled | policy-controlled |
| Export print package | ✓ | ✓ when eligible | ✓ when eligible |
| Delete/archive jobs | ✓ | archive if allowed |  |
| View audit log | ✓ | own job activity | review-related |
| Change security settings | ✓ |  |  |

## Authorization rules

1. Authorization is checked in the privileged desktop/service layer, not only in UI.
2. Tool adapters receive authorized commands, not user-supplied arbitrary execution payloads.
3. Preflight overrides require a reason and AuditEvent.
4. Destructive overwrite/delete requires explicit user confirmation regardless of role unless a future policy explicitly permits automated cleanup.
