# LAB-001: Configure a Host to Attempt IPSec Negotiation

## Project summary

This Windows security administration lab documents a local IPSec transport-mode policy that attempts to negotiate protected communications while allowing unsecured fallback when negotiation fails.

The exercise is valuable because it shows that selecting encryption options does not automatically guarantee that every connection is encrypted. Policy behavior depends on both the negotiated security method and the configured fallback path.

## Environment

- Windows Server 2019 lab host
- Local Security Policy console
- IP Security Policies on Local Computer
- Host-to-host IPSec transport mode

## Configuration design

| Control | Lab setting |
| --- | --- |
| Policy | `Structureality IPSec Policy (attempt)` |
| Traffic scope | Mirrored, any source, any destination, any protocol |
| Network type | All network connections |
| Tunnel endpoint | None; transport mode |
| Filter action | `Encrypt Some of The Things` |
| Security behavior | Negotiate security |
| Fallback | Allow unsecured communication if negotiation fails |
| Protection when negotiated | Integrity and encryption |
| Authentication | Lab-provided preshared key, omitted from documentation |

## What I practiced

- Creating a local IPSec policy and security rule
- Building a mirrored Any-to-Any IP filter
- Selecting transport mode rather than tunnel mode
- Configuring negotiation, integrity, and encryption
- Analyzing the risk introduced by plaintext fallback
- Assigning, validating, and unassigning the policy

## Expected validation

- The named policy appears in Local Security Policy.
- The rule references the correct filter list and filter action.
- The policy can be assigned and validated successfully.
- The policy can be unassigned to restore the baseline state.
- A later traffic test should distinguish successfully negotiated ESP traffic from permitted unencrypted fallback traffic.

## Security takeaway

`Negotiate security` plus `Allow unsecured communication if a secure connection cannot be established` prioritizes compatibility and availability. It does **not** provide a fail-closed encryption guarantee. For sensitive traffic, a mandatory policy or another strongly enforced secure channel is more appropriate.

## Production considerations

- Replace preshared keys with centrally managed authentication such as certificates or Kerberos where appropriate.
- Narrow broad Any-to-Any filters to the smallest required scope.
- Validate traffic on the wire; do not assume a configured rule is actively protecting every session.
- Monitor policy failures and fallback events.

## Skills demonstrated

Windows security administration, IPSec transport mode, IP filtering, policy negotiation, authentication configuration, fallback-risk analysis, and technical validation.
