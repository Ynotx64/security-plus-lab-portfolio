# Security+ Administration Lab Portfolio

Hands-on Windows and Linux security administration case studies covering IPSec policy design, backup and recovery, and media sanitization.

These projects emphasize a repeatable workflow:

1. Define the security objective and scope.
2. Configure the control in an isolated lab.
3. Identify the expected system state.
4. Validate behavior rather than relying on configuration alone.
5. Document security implications, limitations, and production considerations.

## Projects

| Project | Platform | Focus | Primary takeaway |
| --- | --- | --- | --- |
| [LAB-001: Attempt IPSec negotiation](labs/lab-001-ipsec-negotiate/README.md) | Windows Server | Transport-mode IPSec with unsecured fallback | Negotiation does not guarantee encryption when plaintext fallback is allowed. |
| [LAB-002: Require IPSec protection](labs/lab-002-ipsec-require/README.md) | Windows Server | Fail-closed transport security | Requiring integrity and encryption prevents incompatible peers from silently falling back to plaintext. |
| [LAB-003: Backup and file recovery](labs/lab-003-backup-recovery/README.md) | Windows Server | Backup, restoration, and VSS | A backup is only useful when recovery is tested and verified. |
| [LAB-004: Drive sanitization](labs/lab-004-drive-sanitization/README.md) | Kali Linux | Recovery, shredding, formatting, and zeroization | Deletion and formatting are not equivalent to verified sanitization. |

## Skills demonstrated

- Windows Local Security Policy and IPSec rule administration
- IP filter lists, filter actions, transport mode, and authentication selection
- Security trade-off analysis: availability versus fail-closed protection
- Windows Server Backup and point-in-time file recovery
- Volume Shadow Copy Service and previous-version restoration
- Linux disk discovery, partitioning, formatting, and mounting
- TestDisk recovery analysis
- File-level shredding and whole-device zeroization
- Evidence-driven verification and technical documentation

## Security and publishing notes

- Credentials, preshared keys, and lab secrets are intentionally omitted.
- Device names and commands are presented as lab examples; destructive storage commands must never be run against an unverified device.
- Screenshots are optional supporting material. The authoritative checks are the expected result and verification condition documented for each step.
- Production environments should use organization-approved authentication, backup, and media-sanitization standards.

## LinkedIn drafts

Copy-ready LinkedIn posts are available in [`linkedin-posts/`](linkedin-posts/README.md).
