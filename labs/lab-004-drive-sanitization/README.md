# LAB-004: Perform Drive Sanitization and Recovery Testing

## Project summary

This Kali Linux lab compares ordinary deletion, file recovery, file-level shredding, filesystem formatting, and whole-device zeroization.

The core objective is to verify how much data remains recoverable after each technique and to distinguish data removal from sanitization.

## Environment

- Kali Linux virtual machine
- Separate 80 GiB lab storage device
- `fdisk`, `mkfs`, `mount`, `TestDisk`, `shred`, and `dd`
- FAT-formatted removable-media simulation

## Workflow

1. Identify the correct non-OS storage device.
2. Create and format a primary partition.
3. Mount the partition and copy test data.
4. Delete a directory and recover it with TestDisk.
5. Shred files and test whether recovered-looking files retain content.
6. Reformat the partition and test recovery again.
7. Overwrite the entire device with zeros and verify that no partition remains.

## What I practiced

- Linux device discovery and partition management
- Filesystem creation and mounting
- Deleted-file recovery with TestDisk
- Recursive file discovery and secure deletion with `shred`
- Formatting versus sanitization analysis
- Whole-device zeroization with `dd`
- Post-sanitization verification

## Expected validation

- The correct 80 GiB non-OS target is identified before destructive commands.
- Ordinary deletion leaves recoverable data.
- Shredded files may retain names or metadata but not their original content.
- Formatting prevents recovery with the tested tool but does not prove sanitization.
- Whole-device zeroization removes the partition table and TestDisk finds no partitions.

## Security takeaway

Deletion normally removes references, not data. Formatting rebuilds filesystem structures but may leave underlying data recoverable. File-level overwriting provides stronger protection on suitable media, while full-device sanitization addresses the entire addressable device.

## Production considerations

- Verify the target device immediately before `mkfs` or `dd`; a wrong device path causes irreversible loss.
- Do not assume `shred` reliably sanitizes SSDs, snapshots, or copy-on-write filesystems.
- Use approved device sanitize commands or cryptographic erase for modern storage when appropriate.
- Select the sanitization method based on media type, data classification, reuse plans, and organizational policy.

## Skills demonstrated

Linux storage administration, partitioning, file recovery, secure deletion, zeroization, recovery verification, and media-sanitization risk analysis.
