# Security Review Checklist

Use at M11 and release candidate.

## Desktop
- [ ] nodeIntegration disabled in renderer
- [ ] contextIsolation enabled
- [ ] preload exposes minimum typed surface
- [ ] CSP configured
- [ ] no unsafe dynamic eval
- [ ] external navigation/window opening restricted

## Secrets
- [ ] API/OAuth secrets OS-protected
- [ ] renderer never receives raw secret
- [ ] logs/support bundles redact secrets
- [ ] no secrets in Git history/test fixtures
- [ ] disconnect/revoke supported

## IPC / local bridge
- [ ] allow-listed IPC
- [ ] schema validation
- [ ] authorization at privileged boundary
- [ ] bridge loopback-only
- [ ] bridge authentication/session rotation
- [ ] version negotiation
- [ ] size/rate/timeout controls
- [ ] unknown commands rejected

## LLM/tool safety
- [ ] no RUN_CODE/RUN_SCRIPT/RUN_SHELL tool
- [ ] no raw model batchPlay path
- [ ] imported content treated as untrusted
- [ ] tool parameters independently validated
- [ ] destructive actions gated
- [ ] iteration/cost limits

## Files
- [ ] path traversal tests
- [ ] safe import/export destinations
- [ ] overwrite confirmation/versioning
- [ ] checksums for production artifacts
- [ ] untrusted executables never auto-launched

## Production integrity
- [ ] Print Engine independent from LLM
- [ ] no print-ready without preflight
- [ ] overrides authorized/audited
- [ ] profile version pinned
- [ ] final physical dimensions recorded

## Supply chain/release
- [ ] lockfile committed
- [ ] dependency audit reviewed
- [ ] installer integrity
- [ ] code signing status documented
- [ ] update channel secure if enabled
- [ ] rollback tested

## Privacy
- [ ] document what content leaves device
- [ ] provider data minimization
- [ ] support bundle review
- [ ] telemetry opt-in/policy consistent with OI-013
