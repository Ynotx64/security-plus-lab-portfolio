# LAB-002: Require IPSec Integrity and Encryption

## Project summary

This lab documents a Windows host IPSec transport-mode policy that requires negotiated security for matching traffic and blocks unsecured fallback.

It builds directly on the compatibility-oriented LAB-001 design and demonstrates the operational difference between attempting encryption and requiring it.

## Environment

- Windows Server 2019 lab host
- Local Security Policy console
- IP Security Policies on Local Computer
- Host-to-host IPSec transport mode

## Configuration design

| Control | Lab setting |
| --- | --- |
| Policy | `Structureality IPSec Policy (required)` |
| Traffic scope | Mirrored, any source, any destination, any protocol |
| Network type | All network connections |
| Tunnel endpoint | None; transport mode |
| Filter action | `Encrypt All The Things` |
| Security behavior | Negotiate security |
| Fallback | Do not allow unsecured communications |
| Protection | Integrity and encryption |
| Authentication | Lab-provided preshared key, omitted from documentation |

## What I practiced

- Creating and naming a local IPSec policy
- Building a bidirectional IP filter list
- Configuring fail-closed transport protection
- Requiring confidentiality and integrity
- Selecting and protecting an authentication method
- Validating assignment state and policy lifecycle

## Expected validation

- The policy and its enabled rule appear in Local Security Policy.
- The rule uses the intended filter list and required filter action.
- Assignment changes the policy state to `Yes`.
- CompTIA scoring validates the required configuration.
- Unassignment returns the environment to its baseline state.
- Packet-level testing should confirm protected ESP traffic when compatible peers communicate.

## Security takeaway

The decisive control is **Do not allow unsecured communications**. If negotiation fails, traffic is blocked instead of silently reverting to plaintext. This strengthens confidentiality and integrity but can affect availability when peers are misconfigured or incompatible.

## Production considerations

- Use certificates or Kerberos instead of shared secrets when feasible.
- Test policy rollout in stages to prevent unintended outages.
- Use scoped filters rather than an unrestricted Any-to-Any rule.
- Monitor Security Associations, negotiation failures, and blocked traffic.

## Skills demonstrated

Windows security administration, fail-closed policy design, IP filters, IPSec negotiation, integrity and encryption controls, authentication, and configuration verification.
