# LAB-003: Implement Backups and Validate Recovery

## Project summary

This Windows Server lab covers the complete recovery path: preparing dedicated backup media, creating a one-time backup, deleting a file, restoring it to its original location, and using Volume Shadow Copy Service to recover a previous file version.

The project treats restoration testing—not backup creation—as the real proof of recoverability.

## Environment

- Windows Server 2019 lab host
- Secondary NTFS volume labeled `Backup01` and mounted as `F:`
- Windows Server Backup
- Volume Shadow Copy Service

## Workflow

1. Prepare an online, writable NTFS backup volume.
2. Create sample files in user and public directories.
3. Back up `C:\Users` to `Backup01 (F:)`.
4. Permanently delete a sample file.
5. Recover the file from Windows Server Backup to its original location.
6. Enable VSS for `C:` and create a shadow copy.
7. Modify a public file and restore its previous version.

## What I practiced

- Disk provisioning and NTFS volume preparation
- Windows Server Backup configuration
- Source and destination validation
- File-level recovery from a point-in-time backup
- Volume Shadow Copy enablement
- Previous-version restoration
- Recovery verification through readable restored content

## Expected validation

- `Backup01 (F:)` is online, writable, and formatted as NTFS.
- The backup job completes without errors.
- The deleted file is absent before recovery and readable afterward.
- VSS displays an initial shadow copy and next-run time.
- The modified file returns to its earlier content after previous-version restoration.
- Lab scoring confirms each recovery task.

## Security takeaway

A successful backup job is not enough. Recovery must be tested. VSS offers convenient local version recovery, but it remains on the source volume and is not a substitute for an independent, protected backup.

## Production considerations

- Use scheduled backups with defined retention and recovery objectives.
- Isolate backup credentials and storage from ordinary user access.
- Maintain offline or immutable copies for ransomware resilience.
- Test restores regularly and document recovery time and recovery point results.

## Skills demonstrated

Windows Server administration, backup-media preparation, Windows Server Backup, point-in-time recovery, VSS configuration, previous-version restoration, and resilience planning.
