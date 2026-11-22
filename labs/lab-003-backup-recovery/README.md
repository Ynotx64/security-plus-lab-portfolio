# LAB-003: Implement Backups and Validate Recovery

> Source of truth: [Notion lab documentation](https://app.notion.com/p/41d8b0da90df81e9b236cb42534ed8c6). The procedure below mirrors the Notion page, including expected results, verification checks, commands, and available evidence.

> **Documentation status:** The complete procedure includes expected outcomes and verification checks. Screenshots are optional and are not required to complete this documentation.

**Date:** November 19, 2026
**Lab:** CompTIA Applied Lab — Implement Backups
**Objective:** Prepare backup media, create sample files, perform a one-time Windows Server Backup, delete and recover a file, and document resilience and recovery controls.

## Environment and scope

- Virtual machine: **PC10**, Windows Server 2019 operating as a client.
- User context: Jaime using the lab-provided credentials. Do not publish the password in screenshots or documentation.
- Backup source: `C:\Users`.
- Backup target: Disk 1, formatted as NTFS, labeled `Backup01`, and assigned drive letter `F:`.
- Sample source file: `C:\SETUP\NetworkMiner\Fingerprints\oui.txt`.
- Recovery test file: `C:\Users\Jaime\Documents\document01.txt`.
- CompTIA objective: 3.4 — Explain the importance of resilience and recovery in security architecture.

<callout icon="ℹ️" color="blue_bg">
The supplied lab text contained duplicated UI artifacts such as `TypeCopycopy`. The commands below are normalized to the valid Windows `copy` command.
</callout>

## Procedure — configuration, expected results, and verification

## Exercise 1 — Prepare for backup operations

### Step 1

Connect to PC10 and sign in as Jaime using the lab-provided credentials.

*Expected result:* The PC10 Windows desktop opens in Jaime’s session.
*Verify:* Confirm the active computer is PC10 and the password is not displayed or recorded.

### Step 2

Create a full-drive NTFS volume on Disk 1 named `Backup01` and assign drive letter `F:`.

```powershell
diskpart
select disk 1
online disk
attributes disk clear readonly
create partition primary
format fs=ntfs label="Backup01" quick
assign letter=f
exit
```

**Evidence — Step 2**

![Step 2 evidence 01](https://github.com/user-attachments/assets/0de5a122-a31a-4179-982a-484473e652cf)

*Expected result:* Disk 1 contains an online, writable NTFS volume labeled `Backup01` with drive letter `F:`.
*Verify:* Use File Explorer, Disk Management, or `diskpart list volume` to confirm the label, filesystem, and drive letter.

<callout icon="⚠️" color="yellow_bg">
Confirm Disk 1 is the unused lab disk before partitioning or formatting. Disk 0 is the operating-system disk.
</callout>

### Step 3

Create three sample files in Jaime's Documents folder.

```powershell
copy C:\SETUP\NetworkMiner\Fingerprints\oui.txt C:\Users\Jaime\Documents\document01.txt
copy C:\SETUP\NetworkMiner\Fingerprints\oui.txt C:\Users\Jaime\Documents\document02.txt
copy C:\SETUP\NetworkMiner\Fingerprints\oui.txt C:\Users\Jaime\Documents\document03.txt
```

**Evidence — Step 3**


*Expected result:* Jaime’s Documents folder contains `document01.txt`, `document02.txt`, and `document03.txt`.
*Verify:* Confirm all three files exist and were copied from the specified source.

### Step 4

Create the public sample file.

```powershell
copy C:\SETUP\NetworkMiner\Fingerprints\oui.txt C:\Users\Public\document04.txt
```

*Expected result:* `C:\Users\Public\document04.txt` exists.
*Verify:* Confirm the file is present and readable.

**Evidence — Step 4**


### Step 5

Close Command Prompt.

### Check your work

- [ ] Disk 1 is online and writable.
- [ ] `Backup01` is formatted with NTFS and mounted as `F:`.
- [ ] Documents 01–03 exist in Jaime's Documents folder.
- [ ] Document 04 exists in the Public folder.
- [ ] CompTIA scoring validates the preparation tasks.

## Exercise 2 — Protect files using Windows Server Backup

### Step 1

Open Windows Server Backup and select **Local Backup**.

*Expected result:* Windows Server Backup opens with Local Backup available.
*Verify:* Confirm the console is running on PC10 and the Backup Once action is available.

**Evidence — Step 1**

![Step 1 evidence 04](https://github.com/user-attachments/assets/93f1ca10-0d28-4be3-be66-9f06d80859c0)

### Step 2

Run a one-time custom backup with these settings:

- Backup option: **Different options**.
- Configuration: **Custom**.
- Items: **Local disk (C:) → Users**.
- Destination type: **Local drives**.
- Destination: **Backup01 (F:)**.

On the Confirmation page, select **Backup** and wait for completion.

*Expected result:* The confirmation page lists `C:\Users` as the source and `Backup01 (F:)` as the destination; the job finishes successfully.
*Verify:* Confirm both paths before starting and confirm the final status reports completion without errors.

**Evidence — Step 2**


### Step 3

After the backup completes, close the Backup Once Wizard.

**Evidence — Step 3**


### Step 4

Minimize Windows Server Backup but leave it open for recovery.

**Evidence — Step 4**

![Step 4 evidence 07](https://github.com/user-attachments/assets/82295753-5a0c-4387-a752-3161a166ed1b)

### Check your work

- [ ] The backup source is `C:\Users`.
- [ ] The backup destination is `Backup01 (F:)`.
- [ ] Windows Server Backup reports successful completion.
- [ ] CompTIA scoring validates the backup task.

## Exercise 3 — Delete and restore a file

### Step 1

Return to PC10 as Jaime.

### Step 2

Delete `document01.txt` from Jaime's Documents folder and empty the Recycle Bin.

**Evidence — Step 2**


*Expected result:* `document01.txt` is absent from Documents and cannot be restored from the Recycle Bin.
*Verify:* Confirm the file is deleted and the Recycle Bin has been emptied.

<callout icon="🧪" color="yellow_bg">
This deletion is intentional within the isolated lab. Emptying the Recycle Bin ensures that recovery must come from the backup.
</callout>

### Step 3

In Windows Server Backup, select **Recover** and use these options:

- Backup location: **This server (PC10)**.
- Backup date: the one-time backup created in Exercise 2.
- Recovery type: **Files and folders**.
- Item: `PC10 → Local disk (C:) → Users → Jaime → Documents → document01.txt`.
- Recovery destination: **Original location**.
- Other recovery options: leave at their defaults.

Select **Recover**, wait for completion, and close the wizard.

*Expected result:* The Recovery Wizard selects `document01.txt`, uses Original location, and completes successfully.
*Verify:* Confirm the correct backup date, file path, destination option, and successful result.

**Evidence — Step 3**


### Step 4

Open the restored `document01.txt` and confirm that it is readable.

*Expected result:* `document01.txt` reappears in Jaime’s Documents folder and opens normally.
*Verify:* Confirm the restored file is present and readable.

**Evidence — Step 4**


### Step 5

Close all lab windows.

### Check your work

- [ ] `document01.txt` was permanently removed before recovery.
- [ ] Recovery used the correct backup date and file path.
- [ ] The file was restored to its original location.
- [ ] The restored file opens successfully.
- [ ] CompTIA scoring validates deletion and recovery.

## Exercise 4 — Enable Volume Shadow Copy Service

### Step 1

Connect to PC10 and sign in as Jaime using the lab-provided credentials.

### Step 2

Enable Volume Shadow Copy Service for drive `C:`:

1. Right-click **Start** and open **Disk Management**.
2. Right-click **(C:)** and select **Properties**.
3. Open the **Shadow Copies** tab.
4. Select \**C:\** and choose **Enable**.
5. Confirm by selecting **Yes**.
6. Verify that the upper pane shows a next-run date and time and that the lower pane lists an immediate shadow copy for the selected volume.
7. Select **OK** and close Disk Management.
   
**Evidence — Step 2**

![Step 2 evidence 11](https://github.com/user-attachments/assets/1851359a-4c0a-4522-b940-bf3434318782)

*Expected result:* Shadow Copies is enabled for C:, a next-run time is displayed, and an initial shadow copy is listed.
*Verify:* Confirm all three indicators on the Shadow Copies tab.

### Step 3

Force VSS to create a shadow copy of drive C:. Open Command Prompt as administrator and run:

`wmic shadowcopy call create Volume=c:\\`

Verify that the command output states **Method execution successful**, then close Command Prompt.

*Expected result:* WMIC reports **Method execution successful** and creates a new shadow copy.
*Verify:* Confirm the success message and, if needed, confirm a newer shadow-copy timestamp.

<callout icon="ℹ️" color="blue_bg">
The forced shadow operation ensures that a usable previous version exists before the file is modified.
</callout>

### Step 4

Modify `C:\Users\Public\document04.txt`:

1. Open File Explorer and navigate to `C:\Users\Public`.
2. Open `document04.txt` in Notepad.
3. Add `changed` as a new first line and press Enter.
4. Close Notepad and save the change.

*Expected result:* `document04.txt` opens with `changed` as its first line.
*Verify:* Confirm the change was saved before attempting previous-version recovery.

### Step 5

Restore the previous version of `document04.txt`:

1. Right-click `document04.txt` and select **Restore previous versions**.
2. On the Previous Versions tab, select the most recent file version.
3. Select **Restore**.
4. Confirm the restore operation.
5. Acknowledge the successful-restore message and close Properties.

*Expected result:* The most recent previous version is available and Windows reports that restoration succeeded.
*Verify:* Confirm the selected timestamp predates the edit and acknowledge the success message.

<callout icon="⚠️" color="yellow_bg">
VSS retains previous versions on the same volume, but it is not an independent backup. If the source volume is lost or unavailable, VSS on that volume cannot provide recovery.
</callout>

### Step 6

Open `document04.txt` and confirm that the first line `changed` is no longer present. Close Notepad.

*Expected result:* The restored `document04.txt` no longer begins with `changed`.
*Verify:* Open the file and confirm its original content has returned.

### Step 7

Leave File Explorer open.

### Check your work

- [ ] Volume Shadow Copy Service is enabled for C:.
- [ ] A shadow copy exists and a next-run time is displayed.
- [ ] The forced WMIC shadow-copy operation completed successfully.
- [ ] `document04.txt` was modified after the shadow copy.
- [ ] The most recent previous version was restored.
- [ ] The restored file no longer contains the added `changed` line.
- [ ] CompTIA scoring validates VSS enablement, file modification, and previous-version recovery.

## Security interpretation

- **Dedicated backup media** separates recovery data from the primary volume, improving resilience against source-drive failure.
- **One-time backup** demonstrates point-in-time protection, while scheduled backups provide stronger operational coverage.
- **Restore testing** proves recoverability; a backup that cannot be restored does not meet the recovery objective.
- **Original-location recovery** returns the file to its expected application and user path.
- **Volume Shadow Copy Service** can provide previous versions for rapid recovery from accidental modification, but it is not a replacement for independent backups.
- **Access control and isolation** remain important because ransomware or privileged attackers may target attached backup media.

## Verification and results

- [ ] Confirm `Backup01 (F:)` exists and is NTFS.
- [ ] Confirm successful CompTIA scores for sample-file creation.
- [ ] Confirm Windows Server Backup completion.
- [ ] Confirm deletion of `document01.txt`.
- [ ] Confirm successful recovery to the original location.
- [ ] Open and validate the restored file.
- [ ] Confirm VSS enabled on C: and the initial shadow copy.
- [ ] Confirm the successful forced WMIC shadow-copy operation.
- [ ] Confirm the modified document04.txt.
- [ ] Confirm previous-version selection and successful restoration.
- [ ] Confirm the restored file no longer contains the added changed line.
- [ ] Record the final CompTIA lab score.

**Result:** The complete procedure is documented with expected outcomes and verification checks. Backup, file-recovery, VSS, previous-version recovery, and scoring remain pending execution verification; screenshots are optional.

## Verification guide

| Checkpoint | Expected result | What to verify |
| --- | --- | --- |
| Backup media | Backup01 (F:) in File Explorer, Disk Management, or diskpart | Disk 1 is online, NTFS, and mounted as F: |
| Sample files | Jaime Documents and Public folders | Documents 01–04 exist in the required paths |
| Backup configuration | Backup Once Wizard confirmation | Source is C:\\Users; destination is Backup01 (F:) |
| Backup result | Completed backup status | Backup finished successfully |
| Deletion test | Jaime Documents after deletion | document01.txt is absent |
| Recovery selection | Recovery Wizard item and destination | Correct backup, file, and original location selected |
| Recovery result | Completed recovery and restored file | document01.txt is present and readable |
| VSS enablement | Shadow Copies tab for C: | C: enabled, next-run time shown, and initial shadow copy listed |
| Forced shadow copy | Administrative Command Prompt | WMIC reports Method execution successful |
| File modification | document04.txt in Notepad | changed appears as the new first line |
| Previous-version restore | Previous Versions selection and confirmation | Most recent version selected and restore succeeds |
| Restored content | document04.txt after restoration | Added changed line is absent |
| Final validation | CompTIA Score result | Pending |

## Portfolio skills demonstrated

Windows Server administration; disk provisioning; NTFS volume preparation; command-line file operations; Windows Server Backup; backup-target selection; point-in-time recovery; file-restoration validation; Volume Shadow Copy Service configuration; previous-version recovery; resilience and recovery planning; evidence-based technical documentation.

## Source

[CompTIA Lab on Demand — Implement Backups](https://labclient.labondemand.com/Instructions/915f3d5a-d67a-4d41-9984-4415411df929)

## Consultant addendum — enterprise application

The workflow supports ransomware resilience, continuity planning, audit evidence, and recovery-objective testing. VSS is a local recovery layer; independent, access-controlled, offline, or immutable backups remain necessary.

```powershell
wbadmin get versions -backupTarget:F:
vssadmin list shadows
vssadmin list shadowstorage
```

**Employer value:** Demonstrates that backup completion is treated as an intermediate state and that a readable, validated restore is the actual control objective.

