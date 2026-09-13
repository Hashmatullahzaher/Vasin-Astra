# Deployment and Windows Packaging

## 1. V1 deployment target

Installable Windows desktop application on Vasin workstation(s).

Exact supported Windows versions are pending OI-001.

## 2. Release artifacts

Expected:
- Windows installer;
- versioned application executable/bundle;
- integrity hash;
- release notes;
- compatibility matrix;
- optional separately packaged Adobe plugin/adapter if required by Adobe distribution model.

## 3. Installer responsibilities

- install application under normal Windows conventions;
- create per-user app-data directories;
- never bundle user API keys;
- initialize database safely on first run;
- preserve data on normal application upgrade;
- clearly distinguish uninstall from deletion of user projects;
- check/guide required creative-app adapter installation.

## 4. Code signing

Production distribution should use a trusted Windows code-signing certificate.

Release is allowed without signing only for an explicitly approved internal pilot; Windows warnings and operational implications must be documented.

## 5. Application updates

Decision pending OI-013.

If auto-update is enabled:
- signed release metadata;
- TLS;
- rollback/recovery strategy;
- never update while a critical file migration/export is in progress;
- database backup before risky migration.

## 6. Adobe adapter deployment

Photoshop UXP plugin packaging/distribution must follow the supported Adobe mechanism for the target host/version.

Illustrator bridge deployment depends on approved OI-004 mechanism.

The desktop installer must not assume third-party app file locations without discovery/version checks.

## 7. Configuration

Per-device non-secret config:
- workspace path;
- UI preferences;
- adapter settings;
- selected models;
- update policy.

Secrets are referenced by ID and live in OS secret storage.

## 8. Release versioning

Use SemVer unless a later decision changes it.

Every print package records application version and adapter versions for traceability.

## 9. Rollback

Before release:
- define how to reinstall prior application version;
- ensure database schema compatibility or restore path;
- preserve user job folders;
- never rely on "uninstall and delete everything" as rollback.
