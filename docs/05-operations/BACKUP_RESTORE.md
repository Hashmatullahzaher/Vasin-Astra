# Backup and Restore

## 1. Backup scope

A valid Vasin Astra backup should cover:
- SQLite database;
- job workspace files;
- printer profiles;
- application configuration that is not secret;
- reference library when enabled.

OS credential-store secrets should not be exported into normal backups.

## 2. Consistency

Do not copy a live SQLite database blindly while writes are active.

Use:
- SQLite online backup API or safe checkpoint/snapshot strategy;
- filesystem manifest/checksums for job files.

## 3. Backup types

### Manual backup
Admin selects destination and creates a timestamped backup bundle/manifest.

### Automatic local recovery
Optional periodic database/recovery snapshots, respecting storage limits.

## 4. Restore

Restore flow must:
1. validate backup manifest/version;
2. verify checksums where available;
3. block restore into an active write session;
4. preserve current data in a safety snapshot unless explicitly waived;
5. restore database and files;
6. run compatible migrations;
7. verify job/source references;
8. report missing external-linked assets.

## 5. Secrets after restore

If restoring to a different Windows account/device:
- credential references may be unresolved;
- user must reconnect OpenAI/Canva/etc.;
- application must not treat missing credentials as corruption of job data.

## 6. Required release test

Before M12:
- create jobs/assets/profile;
- take backup;
- alter/remove local state in test environment;
- restore;
- reopen job;
- verify source/preview/export metadata;
- confirm secret values were not included in backup.
