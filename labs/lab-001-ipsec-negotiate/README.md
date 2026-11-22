# LAB-001: Configure a Host to Attempt IPSec Negotiation

> Source of truth: [Notion lab documentation](https://app.notion.com/p/41d8b0da90df81938057c69d70ca0aec). The procedure below mirrors the Notion page, including expected results, verification checks, commands, and available evidence.

**Host:** PC10-v2023-06 (10 IPSEC VM10)
**Documentation status:** Every procedure step includes an expected outcome and verification check. Screenshots are optional and are not required.
**Security distinction:** Attempt negotiated IPSec; permit unsecured fallback if negotiation fails.

## Executive summary

Configure a Windows local IPSec policy that negotiates transport-mode protection for broadly matching IP traffic. The key policy decision allows plaintext fallback, unlike mandatory IPSec. This is a compatibility-oriented teaching scenario, not an appropriate fail-closed setting for sensitive enterprise traffic.

## Environment and security configuration

| Setting | Value |
| --- | --- |
| Management console | Local Security Policy → IP Security Policies on Local Computer |
| Policy | Structureality IPSec Policy (attempt) |
| Filter list | Structureality IP Filter List |
| Action | Encrypt Some of The Things |
| Scope | All network connections; mirrored; any source, destination and protocol |
| Mode | Transport (no tunnel endpoint) |
| Negotiation | Negotiate security |
| Fallback | Allow unsecured if secure connection cannot be established |
| Protection when negotiated | Integrity and encryption |
| Authentication | Lab-provided preshared key, never publish actual secret |

## Procedure — expected results and verification

### Step 1

Connect to PC10-v2023-06 (10 IPSEC VM10); sign in as Admin using provided lab credentials.

*Expected result:* The Windows desktop opens in the authorized Admin session.
*Verify:* Confirm the active virtual machine is PC10-v2023-06 before changing local security policy.

### Step 2

Search for local security; open Local Security Policy.

*Expected result:* Windows Search returns Local Security Policy and the management console opens.
*Verify:* Confirm the console title is Local Security Policy and Security Settings is visible.

### Step 3

Select IP Security Policies on Local Computer.

*Expected result:* IP Security Policies on Local Computer becomes selected and the right pane displays available policies.
*Verify:* Confirm the selection is under Security Settings on the local computer.

### Step 4

Right-click the right pane; choose Create IP Security Policy.

*Expected result:* The IP Security Policy Wizard opens to its welcome page.
*Verify:* Confirm the wizard was launched from IP Security Policies on Local Computer.

### Step 5

Select Next on the IP Security Policy Wizard welcome page.

*Expected result:* The wizard advances to the IP Security Policy Name page.
*Verify:* Confirm the Name and Description fields are available.

### Step 6

Set policy name to **Structureality IPSec Policy (attempt)**; Next.

*Expected result:* The policy name is set to Structureality IPSec Policy (attempt).
*Verify:* Confirm the exact name includes the attempt designation before continuing.

### Step 7

Leave Requests for Secure Communication unchecked; Next.

*Expected result:* The wizard advances without creating a default response rule.
*Verify:* Confirm Requests for Secure Communication remains unchecked.

### Step 8

Ensure Edit properties is checked; Finish.

*Expected result:* The policy is created and its Properties window opens.
*Verify:* Confirm Edit properties was checked and the Rules tab is available.

### Step 9

In policy Properties select Add.

*Expected result:* The Create IP Security Rule Wizard opens.
*Verify:* Confirm the rule is being added to Structureality IPSec Policy (attempt).

### Step 10

Select Next on Create IP Security Rule Wizard welcome.

*Expected result:* The wizard advances to the Tunnel Endpoint page.
*Verify:* Confirm the tunnel choices are displayed.

### Step 11

Select This rule does not specify a tunnel; Next.

*Expected result:* The wizard advances to Network Type with no tunnel endpoint configured.
*Verify:* Confirm This rule does not specify a tunnel is selected, establishing transport mode.

### Step 12

Select All network connections; Next.

*Expected result:* The wizard advances to the IP Filter List page.
*Verify:* Confirm All network connections is selected.

### Step 13

On IP Filter List select Add.

*Expected result:* The IP Filter List dialog opens for creating a new filter list.
*Verify:* Confirm the Add action is available.

### Step 14

Name list **Structureality IP Filter List**; Add.

*Expected result:* A filter list named Structureality IP Filter List is created and the IP Filter Wizard starts.
*Verify:* Confirm the exact list name and that Use Add Wizard is checked.

### Step 15

Select Next on IP Filter Wizard welcome.

*Expected result:* The wizard advances to the description and mirrored-property page.
*Verify:* Confirm the filter is being added to Structureality IP Filter List.

### Step 16

Leave Description empty; confirm Mirrored checked; Next.

*Expected result:* The wizard advances to IP Traffic Source with bidirectional matching enabled.
*Verify:* Confirm Description is blank and Mirrored is checked.

### Step 17

Select Any IP Address for source; Next.

*Expected result:* The wizard advances to IP Traffic Destination.
*Verify:* Confirm IP Traffic Source is set to Any IP Address.

### Step 18

Select Any IP Address for destination; Next.

*Expected result:* The wizard advances to IP Protocol Type.
*Verify:* Confirm IP Traffic Destination is set to Any IP Address.

### Step 19

Select Any for IP Protocol Type; Next.

*Expected result:* The wizard advances to the completion page.
*Verify:* Confirm IP Protocol Type is set to Any.

### Step 20

Leave Edit properties unchecked; Finish.

*Expected result:* The IP Filter Wizard closes and the new Any-to-Any filter appears in the filter list.
*Verify:* Confirm Edit properties is unchecked and the filter is listed.

### Step 21

Confirm new filter appears; OK.

*Expected result:* The filter-list dialog closes and the Security Rule Wizard displays available filter lists.
*Verify:* Confirm Structureality IP Filter List is present.

### Step 22

Select Structureality IP Filter List radio button; Next.

*Expected result:* The wizard advances to the Filter Action page.
*Verify:* Confirm Structureality IP Filter List is selected.

### Step 23

Ensure Use Add Wizard checked; Add.

*Expected result:* The IP Security Filter Action Wizard opens.
*Verify:* Confirm Use Add Wizard is checked before selecting Add.

### Step 24

Select Next on IP Security Filter Action Wizard welcome.

*Expected result:* The wizard advances to the Filter Action Name page.
*Verify:* Confirm the name field is ready for the new action.

### Step 25

Set name to **Encrypt Some of The Things**; Next.

*Expected result:* The action is named Encrypt Some of The Things and the wizard advances to general options.
*Verify:* Confirm the exact action name before continuing.

### Step 26

Select Negotiate security; Next.

*Expected result:* The wizard advances to the unsecured-communication behavior page.
*Verify:* Confirm Negotiate security is selected.

### Step 27

Select **Allow unsecured communication if a secure connection cannot be established**; Next.

*Expected result:* The wizard advances to IP Traffic Security methods.
*Verify:* Confirm Allow unsecured communication if a secure connection cannot be established is selected.

### Step 28

Select Integrity and encryption; Next.

*Expected result:* The wizard advances to its completion page.
*Verify:* Confirm Integrity and encryption is selected.

### Step 29

Leave Edit properties unchecked; Finish.

*Expected result:* The action wizard closes and Encrypt Some of The Things appears in the Filter Actions list.
*Verify:* Confirm Edit properties is unchecked and the new action is listed.

### Step 30

Select Encrypt Some of The Things filter action; Next.

*Expected result:* The wizard advances to Authentication Method.
*Verify:* Confirm Encrypt Some of The Things is selected.

### Step 31

Choose preshared-key authentication; enter the lab-provided shared secret; Next. Do not publish the key.

*Expected result:* Preshared-key authentication is accepted and the wizard advances to completion.
*Verify:* Confirm the preshared-key option remains selected, but do not display or record the secret.

### Step 32

Leave Edit properties unchecked; Finish.

*Expected result:* The Security Rule Wizard finishes and returns to policy Properties.
*Verify:* Confirm Edit properties is unchecked before selecting Finish.

### Step 33

Confirm Structureality IP Filter List rule is checked in policy Properties; OK.

*Expected result:* The completed security rule appears in policy Properties and is enabled.
*Verify:* Confirm its checkbox is selected and it references Structureality IP Filter List and Encrypt Some of The Things.

### Step 34

Confirm Structureality IPSec Policy (attempt) appears in Local Security Policy.

*Expected result:* Structureality IPSec Policy (attempt) appears in the Local Security Policy right pane.
*Verify:* Confirm the exact policy name and its initial assignment state.

### Step 35

Right-click policy; Assign; select Score to validate.

*Expected result:* The policy assignment state changes to Yes and the CompTIA Score validation succeeds.
*Verify:* Confirm the correct policy is assigned and the score reports success.

### Step 36

Right-click policy; Un-assign to establish baseline for subsequent capture exercise.

*Expected result:* The policy assignment state returns to No.
*Verify:* Confirm the policy is unassigned to establish the plaintext baseline for the later capture exercise.

### Step 37

Leave Local Security Policy open.

*Expected result:* Local Security Policy remains open with the completed policy visible.
*Verify:* Confirm the console is left ready for the next lab activity.

## Validation

- [ ] Confirm the named policy exists on PC10.
- [ ] Confirm mirrored Any-to-Any filter and selected Encrypt Some of The Things action.
- [ ] Verify fallback option allows unsecured communications when negotiation fails.
- [ ] Verify Assigned = Yes and confirm a successful Score at Step 35.
- [ ] Verify Assigned returns to No after Step 36.
- [ ] During the later exercise, compare traffic with and without IPSec negotiation and distinguish ESP traffic from unencrypted application traffic.

## Security analysis

The combination of Negotiate security and Allow unsecured communication if a secure connection cannot be established permits communication even if IPSec fails. The system therefore does not guarantee encryption. A mirrored Any-to-Any filter describes broad matching traffic but does not alone verify protection. This rule specifies **transport mode**, not an IPSec tunnel endpoint. Preshared keys should be redacted in public evidence and replaced with stronger centrally managed methods where appropriate in production.

## Results and verification status

**Pending execution verification.** The procedure now documents the expected state and verification requirement for every step. Policy assignment, the Score result, and packet-level behavior still require execution checks. Screenshots are optional. The prescribed Step 36 ends with the policy unassigned.

## Employer-facing competencies

Windows local security policy administration, IP filters, traffic scoping, IPSec negotiation, transport-mode configuration, authentication, encrypted versus unsecured fallback risk analysis, policy lifecycle, lab validation and technical reporting.

## Related resources

- [Original LAB-001 instructions](https://labclient.labondemand.com/Instructions/f758d96e-b047-4c85-816c-c4007fb38668)
- [LAB-002 — Configure a host to require an IPSec VPN](https://app.notion.com/p/41d8b0da90df81228f30f6874be84b04)

## Consultant addendum — enterprise application

This compatibility-oriented policy can support a controlled migration while legacy peers are remediated. In production, unsecured fallback should be time-bound, narrowly scoped, monitored, and governed as an exception.

```powershell
Get-NetIPsecRule
Get-NetIPsecMainModeSA
Get-NetIPsecQuickModeSA
```

**Employer value:** Demonstrates the ability to distinguish a configured control from an enforced outcome and to translate plaintext fallback into a clear confidentiality risk.

