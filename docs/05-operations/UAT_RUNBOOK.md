# UAT Runbook

## 1. Participants

At least:
- Vasin operator/designer;
- Vasin print-production reviewer;
- implementation representative.

Record target workstation:
- Windows version;
- Photoshop version;
- Illustrator version;
- adapter versions;
- Vasin Astra version;
- printer/RIP profile used.

## 2. Preconditions

- approved production printer profiles loaded;
- representative logos/photos/fonts available legally;
- API credentials configured;
- real Adobe apps installed and connected;
- clean backup/recovery point.

## 3. UAT-01 Small-format job

Create a business-card-class job.

Validate:
- brief capture;
- final dimensions;
- bleed/safe zone;
- editable text/logo;
- preview;
- natural-language revision;
- preflight;
- editable source;
- requested print export;
- print operator confirms file interprets correctly.

Result: PASS / FAIL + evidence.

## 4. UAT-02 Large-format banner

Create a multi-meter banner (target example 2 m × 30 m or another real Vasin case).

Validate:
- final physical size is correct;
- working scale visible;
- no accidental giant flat bitmap requirement;
- vector text/logo;
- photo effective PPI calculated at final size;
- profile-specific rules;
- print operator understands exported scale/final dimensions;
- source and print file open correctly.

## 5. UAT-03 Low-resolution asset

Use a deliberately insufficient raster.

Expected:
- preflight finding;
- exact effective PPI evidence where calculable;
- no false print-ready state;
- permitted remediation/override behavior matches profile/policy.

## 6. UAT-04 Revision

Instruction example:
"Keep the logo and background. Replace the product photo and move the headline higher."

Expected:
- unchanged elements preserved;
- child revision created;
- new preview;
- audit/history present.

## 7. UAT-05 Tool disconnect

Disconnect/close active host during a controlled test.

Expected:
- error visible;
- no false success;
- reconnect and resume/retry possible;
- job state preserved.

## 8. UAT-06 Restart/recovery

Close/restart Vasin Astra.

Expected:
- jobs persist;
- latest revision/profile/preflight state remains accurate;
- adapters show actual connection state.

## 9. UAT-07 Backup/restore

Run the approved restore exercise.

## 10. Sign-off record

Capture:
- date;
- build SHA/version;
- participants;
- scenario results;
- defects;
- accepted waivers;
- print-production reviewer decision.

Release cannot substitute developer screenshots for actual operator UAT.
