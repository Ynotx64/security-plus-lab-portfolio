# LAB-004: Perform Drive Sanitization and Recovery Testing

> Source of truth: [Notion lab documentation](https://app.notion.com/p/4208b0da90df815b93a2d3736c0afd0f). The procedure below mirrors the Notion page, including expected results, verification checks, commands, and available evidence.

> **Documentation status:** The complete procedure includes expected outcomes and verification checks. Screenshots are optional and are not required to complete this documentation.

**Date:** November 22, 2026
**Lab:** CompTIA Assisted Lab — Performing Drive Sanitization
**Objective:** Prepare portable storage, compare deletion and recovery behavior, securely delete files, test the effects of formatting, and sanitize an entire storage device by overwriting it.

## Environment and scope

- Virtual machine: **KALI**, running Kali Linux.
- User context: root using the lab-provided credentials. Do not publish the password in screenshots or documentation.
- Target: the unpartitioned **80 GiB** lab storage device identified with `fdisk -l`.
- Device placeholder: `&lt;SDB&gt;` represents the verified whole-device path, such as `/dev/sdb`.
- Partition placeholder: `&lt;SDB1&gt;` represents the verified first partition, such as `/dev/sdb1`.
- Mount point: `/mnt/SalesStorage`.
- CompTIA objective: 4.2 — Explain the security implications of proper hardware, software, and data asset management.

<callout icon="⚠️" color="red_bg">
**Destructive-operation boundary:** Commands such as `mkfs` and `dd` permanently overwrite data. Before every destructive command, verify the 80 GiB lab device with `fdisk -l`. Never substitute the Kali operating-system disk or an unverified device path.
</callout>

<callout icon="ℹ️" color="blue_bg">
The supplied lab text contained interface artifacts such as `Typecp`, `Typefind`, and `Typedd`. The commands below are normalized to valid Linux commands.
</callout>

## Procedure — configuration, expected results, and verification

## Exercise 1 — Prepare a portable drive

### Step 1

Connect to KALI and sign in as root using the lab-provided credentials. Open and maximize Terminal Emulator.

### Step 2

Identify the unpartitioned 80 GiB storage device:

```bash
fdisk -l
```

Record the verified device path as `&lt;SDB&gt;`.

*Expected result:* `fdisk -l` lists an unpartitioned 80 GiB device distinct from the operating-system disk.
*Verify:* Confirm the whole-device path and record it as `&lt;SDB&gt;` before running any destructive command.

**Evidence — Step 2**

![Step 2 evidence 01](https://github.com/user-attachments/assets/d3e0661f-38b6-425c-8c76-8443625bda10)

### Step 3

Start `fdisk` against the verified target:

```bash
fdisk <SDB>
```

Use the interactive commands in this order:

1. `m` — display help.
2. `p` — print the current partition table.
3. `v` — verify that the space is unallocated.
4. `n` — create a new partition.
5. `p` — select a primary partition.
6. `1` — choose partition number 1.
7. Press Enter for the default first sector.
8. Press Enter for the default last sector.
9. `p` — confirm the new partition.
10. `w` — write changes and exit.

Record the resulting partition path as `&lt;SDB1&gt;`.

*Expected result:* The partition table lists partition 1 using the expected range of sectors.
*Verify:* Confirm the partition path is `&lt;SDB1&gt;` and the operating-system disk was not modified.

**Evidence — Step 3**


### Step 4

Format the new partition for cross-platform use:

```bash
mkfs -t fat <SDB1>
```

*Expected result:* `mkfs` completes without errors and creates the lab-required FAT filesystem.
*Verify:* Confirm the command targeted `&lt;SDB1&gt;`, not the whole device or OS disk.

**Evidence — Step 4**


### Step 5

Create a mount point and mount the partition:

```bash
mkdir /mnt/SalesStorage
mount <SDB1> /mnt/SalesStorage
ls -l /mnt/SalesStorage
```

### Step 6

Copy the SecLists content to the mounted storage and verify it:

```bash
cp -r /usr/share/seclists/* /mnt/SalesStorage/
ls -l /mnt/SalesStorage
```

*Expected result:* `/mnt/SalesStorage` contains the copied SecLists directories and files.
*Verify:* Confirm the partition is mounted and the directory listing is populated.

**Evidence — Step 6**


### Check your work

- [ ] The correct 80 GiB target device was identified.
- [ ] A primary partition using the available space was created.
- [ ] The partition was formatted with the lab-required FAT filesystem.
- [ ] The partition was mounted at `/mnt/SalesStorage`.
- [ ] SecLists content was copied successfully.
- [ ] CompTIA scoring validates the preparation tasks.

## Exercise 2 — Delete and recover files

### Step 1

Delete the Miscellaneous directory and verify that it is absent:

```bash
rm -r /mnt/SalesStorage/Miscellaneous
ls -l /mnt/SalesStorage
```

*Expected result:* `Miscellaneous` no longer appears under `/mnt/SalesStorage`.
*Verify:* Confirm the command removed the directory from the mounted partition.

**Evidence — Step 1**

![Step 1 evidence 05](https://github.com/user-attachments/assets/aa204a4c-62b8-409f-a419-113507233142)

### Step 2

Attempt recovery with TestDisk:

```bash
testdisk <SDB1>
```

**Evidence — Step 2**


Use this workflow:

1. Select **Proceed**.
2. Select partition-table type **None**.
3. Select the FAT partition and choose **Undelete**.
4. Highlight **Miscellaneous**.
5. Press `:` to select it.
6. Press capital `C` to copy.
7. Select `Downloads` as the destination and press capital `C`.
8. Confirm that TestDisk reports **Copy done!**.
9. Press `Ctrl+C` to exit.

**Evidence — Step 2**


### Step 3

Verify the recovered directory:

```bash
ls -l Downloads/Miscellaneous
```

If the terminal display is corrupted after TestDisk, run `reset` and repeat the listing.

*Expected result:* TestDisk reports **Copy done!** and `Downloads/Miscellaneous` contains recovered items.
*Verify:* Confirm the recovered directory and files are readable.

**Evidence — Step 3**


### Check your work

- [ ] The directory was deleted outside the desktop Trash.
- [ ] TestDisk located the deleted directory.
- [ ] The directory was copied to Downloads.
- [ ] The recovered contents are visible.
- [ ] CompTIA scoring validates deletion and recovery.

## Exercise 3 — Securely delete files with shred

### Step 1

Inspect the Passwords directory and a sample file:

```bash
ls -l /mnt/SalesStorage
ls -lR /mnt/SalesStorage/Passwords
cat /mnt/SalesStorage/Passwords/bt4-password.txt
```

*Expected result:* `bt4-password.txt` exists and displays readable original content before shredding.
*Verify:* Confirm the correct source file and content so the later sanitization test has a baseline.

**Evidence — Step 1**


### Step 2

Find and shred every regular file under Passwords:

```bash
find /mnt/SalesStorage/Passwords -type f -exec shred -uvz {} \;
```

This invokes `shred` on each file, uses verbose output, removes the file, and performs a final zeroing pass.

*Expected result:* `shred` processes each regular file, overwrites it, performs the zeroing pass, and removes it.
*Verify:* Confirm the command completes without errors and the files are absent afterward.

**Evidence — Step 2**


### Step 3

Verify that files are gone, then remove the empty directory structure:

```bash
ls -lR /mnt/SalesStorage/Passwords
rm -r /mnt/SalesStorage/Passwords
```

**Evidence — Step 3**


### Step 4

Attempt recovery with TestDisk:

```bash
testdisk <SDB1>
```

**Evidence — Step 4**


Use the same TestDisk workflow, select **Passwords**, and copy it to `Downloads`. Exit TestDisk and inspect the result:

```bash
ls -l Downloads/Passwords
cat Downloads/Passwords/bt4-password.txt
```

The names or directory structure may appear recoverable, but the sample file should contain no original data.

*Expected result:* TestDisk may recover names or directory structure, but the sampled file contains no original data.
*Verify:* Confirm `cat Downloads/Passwords/bt4-password.txt` does not reveal the original content.

**Evidence — Step 4**


<callout icon="⚠️" color="yellow_bg">
`shred` is most dependable on traditional directly addressed storage. Copy-on-write filesystems, snapshots, journaling, SSD wear-leveling, and remapped blocks can retain data outside the overwritten logical file. Use platform-appropriate cryptographic erase or device sanitize commands for real SSD disposal.
</callout>

### Check your work

- [ ] Original file content was confirmed before shredding.
- [ ] Files were processed with the required shred options.
- [ ] Recovery was attempted.
- [ ] Recovered-looking files contain no original data.
- [ ] CompTIA scoring validates the secure-deletion task.

## Exercise 4 — Format a drive and test recovery

### Step 1

Unmount the storage and reformat the partition:

```bash
umount /mnt/SalesStorage
mkfs -t fat <SDB1>
```

*Expected result:* The partition unmounts and `mkfs` creates a fresh FAT filesystem without errors.
*Verify:* Confirm the correct partition was reformatted and is no longer mounted during the operation.

**Evidence — Step 1**


### Step 2

Attempt recovery:

```bash
testdisk <SDB1>
```

Select **Proceed**, choose partition-table type **None**, select the FAT partition, and choose **Undelete**. The expected TestDisk result is **No file found. Filesystem may be damaged**. Press `Ctrl+C` to exit.

*Expected result:* TestDisk reports **No file found. Filesystem may be damaged** for the formatted partition.
*Verify:* Confirm this is a tool-specific recovery result, not proof of secure sanitization.

**Evidence — Step 2**


<callout icon="ℹ️" color="blue_bg">
Failure to recover data with one tool does not prove secure destruction. Formatting recreates filesystem structures but may leave underlying data recoverable with other tools or specialized services.
</callout>

### Check your work

- [ ] The partition was unmounted before formatting.
- [ ] The partition was formatted.
- [ ] TestDisk recovery was attempted.
- [ ] The expected no-files result was recorded.
- [ ] CompTIA scoring validates the formatting task.

## Exercise 5 — Sanitize the entire drive

### Step 1

Place data on the reformatted partition again:

```bash
mount <SDB1> /mnt/SalesStorage
cp -r /usr/share/seclists/* /mnt/SalesStorage/
```

*Expected result:* The reformatted partition is mounted again and contains newly copied SecLists data.
*Verify:* Confirm data is present before the whole-device overwrite begins.

**Evidence — Step 1**


### Step 2

After re-verifying the target, overwrite the entire device with zeros:

```bash
dd if=/dev/zero of=<SDB> bs=1M status=progress
```

Wait for completion.

*Expected result:* `dd` shows progress and completes after writing across the entire verified 80 GiB device.
*Verify:* Confirm `of=` targeted `&lt;SDB&gt;` and the final byte count is consistent with the device size.

<callout icon="⚠️" color="red_bg">
**Final target check:** The `of=` device is destroyed without confirmation. Verify that `&lt;SDB&gt;` is the 80 GiB lab storage device immediately before running `dd`.
</callout>

**Evidence — Step 2**


### Step 3

Confirm that the partition table is gone:

```bash
fdisk <SDB>
```

Inside `fdisk`, enter `p`, then `v`. Confirm that no partitions remain and the space is unallocated. Enter `q` to exit.

*Expected result:* `fdisk` shows no partition table entries and reports the device space as unallocated.
*Verify:* Confirm the former partition is gone and exit without writing new changes.

**Evidence — Step 3**


### Step 4

Attempt device-level recovery:

```bash
testdisk <SDB>
```

Use this workflow:

1. Select **Proceed**.
2. Select partition-table type **Intel**.
3. Select **Analyse**.
4. Confirm that TestDisk reports no partitions.
5. Record the CompTIA Score result.
6. Press `Ctrl+C` to exit.

*Expected result:* TestDisk Analyse finds no partitions and the CompTIA score validates the sanitization task.
*Verify:* Confirm both the no-partitions result and the successful score.

### Check your work

- [ ] Data was placed on the target before sanitization.
- [ ] The entire verified target device was overwritten.
- [ ] The partition table no longer exists.
- [ ] TestDisk analysis found no partitions.
- [ ] CompTIA scoring validates the sanitization task.

## Security interpretation

- **Deletion** removes directory references and marks storage as reusable; it does not normally overwrite file data.
- **Undeletion** can recover data while its storage blocks remain intact.
- **File-level shredding** overwrites logical file content, but metadata and storage-platform behavior can limit assurance.
- **Formatting** rebuilds filesystem structures and is not, by itself, a reliable sanitization method.
- **Whole-device overwriting** provides stronger assurance for magnetic media when the entire addressable device is covered.
- **Cryptographic erase and device sanitize commands** may be more appropriate for modern SSDs, encrypted media, or remapped storage.
- Sanitization method selection should consider data sensitivity, media type, reuse or disposal plans, legal requirements, and required verification.

## Verification and results

- [ ] Record the verified values for `&lt;SDB&gt;` and `&lt;SDB1&gt;`.
- [ ] Confirm successful partition creation, formatting, mounting, and file copy.
- [ ] Confirm ordinary deletion and successful TestDisk recovery.
- [ ] Confirm shred execution and the absence of original recovered content.
- [ ] Confirm reformatting and TestDisk's no-files result.
- [ ] Confirm full-device `dd` completion.
- [ ] Confirm the absence of a partition table and TestDisk's no-partitions result.
- [ ] Record successful CompTIA scores for all exercises.

**Result:** The complete procedure is documented with expected outcomes and verification checks. Configuration, recovery, shredding, formatting, sanitization, and scoring remain pending execution verification; screenshots are optional.

## Verification guide

| Checkpoint | Expected result | What to verify |
| --- | --- | --- |
| Target identification | fdisk -l | Correct unpartitioned 80 GiB device identified |
| Partition creation | fdisk p after n | Partition 1 occupies the intended space |
| FAT filesystem | mkfs result | Filesystem creation succeeds on the verified partition |
| Mount and copy | ls -l /mnt/SalesStorage | SecLists content is present |
| Ordinary deletion | Listing after rm | Miscellaneous is absent |
| Ordinary recovery | TestDisk and Downloads listing | Miscellaneous is recoverable |
| Pre-shred content | cat bt4-password.txt | Original content is readable |
| Shred operation | Verbose shred output | Each regular file is processed |
| Post-shred recovery | Recovered listing and cat output | Names may remain; original content is absent |
| Format test | TestDisk after mkfs | No files found with the tested tool |
| Whole-drive overwrite | dd progress and completion | Entire verified device is overwritten |
| Partition verification | fdisk p and v | No partition table remains |
| Sanitization recovery test | TestDisk Analyse | No partitions are found |
| Final validation | CompTIA Score results | Pending |

## Portfolio skills demonstrated

Linux storage administration; device identification; fdisk partitioning; FAT filesystem creation; mounting; command-line file management; TestDisk recovery; secure file deletion; zeroization; filesystem reformatting; whole-drive sanitization; recovery verification; media-sanitization risk analysis; evidence-based technical documentation.

## Source

[CompTIA Lab on Demand — Performing Drive Sanitization](https://labclient.labondemand.com/Instructions/70acf91b-1050-47e2-90c6-9cb230991bcb)

## Consultant addendum — enterprise application

Sanitization controls apply to reassignment, offboarding, lease returns, incident response, disposal, and regulated-data handling. The method must match the media; SSDs may require vendor sanitize functions or cryptographic erase.

```bash
lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINTS,MODEL
sudo testdisk /dev/sdb1
sudo find /mnt/SalesStorage/Passwords -type f -exec shred -uvz {} \;
sudo dd if=/dev/zero of=/dev/sdb bs=1M status=progress conv=fsync
```

> **Safety:** `mkfs`, `shred`, and `dd` can cause irreversible data loss. Reconfirm the isolated lab device immediately before execution.

**Employer value:** Demonstrates data-remanence analysis, careful use of destructive tools, and recovery-based verification of sanitization outcomes.

