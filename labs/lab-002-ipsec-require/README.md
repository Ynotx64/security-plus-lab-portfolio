# LAB-002: Require IPSec Integrity and Encryption

> Source of truth: [Notion lab documentation](https://app.notion.com/p/41d8b0da90df81228f30f6874be84b04). The procedure below mirrors the Notion page, including expected results, verification checks, commands, and available evidence.

> **Documentation status:** Genuine supplied screenshots remain embedded at their matching steps. Steps without screenshots now include expected outcomes and verification checks; additional screenshots are optional and not required. Final policy assignment, scoring, and on-wire encryption still require execution verification.

**Date:** November 19, 2026
**Lab:** CompTIA Lab 10 — Using IPSec
**Objective:** Create a Windows host IPSec transport-mode policy that requires negotiated encrypted and authenticated communications for matching traffic and disallows unsecured fallback.

## Environment and scope

- Lab instructions specify **PC20**. Supplied screenshots display the lab header **PC10-v2023-06 (10 IPSEC VM10)**. Verify which host actually received the configuration before representing it as PC20.
- Management console: Local Security Policy → IP Security Policies on Local Computer.
- Policy: Structureality IPSec Policy (required).
- Filter list: Structureality IP Filter List.
- Filter action: Encrypt All The Things.
- Traffic scope: mirrored; source Any IP Address; destination Any IP Address; protocol Any; all network connections.
- Rule: no explicit tunnel endpoint (IPSec transport mode).
- Security: negotiate security; no unsecured communication fallback; integrity and encryption; lab-supplied preshared-key authentication.

## Procedure — steps 1–37 with expected results and verification

### Step 1

Connect to PC20 and sign in as Admin; do not record the lab password in public documentation.

*Expected result:* The Windows desktop opens in the authorized administrative session.
*Verify:* Confirm the active VM identity before configuring the policy, because the lab instructions name PC20 while supplied screenshots show PC10.

### Step 2

Search for and open Local Security Policy.

**Screenshot 01 — Local Security Policy search** — `Screenshot 2026-11-19 at 1.58.22 PM.png`

![Step 2 evidence 01](https://github.com/user-attachments/assets/c5c85c35-6084-4b69-994e-20995b25bf36)

*Evidence:* Windows Search shows Local Security Policy as the best match.

### Step 3

Select IP Security Policies on Local Computer.

*Evidence:* IP Security Policies on Local Computer is visibly selected in Screenshot 02 under Step 4.

### Step 4

Right-click the right pane and select Create IP Security Policy.

**Screenshot 02 — Create IP Security Policy** — `Screenshot 2026-11-19 at 1.58.59 PM.png`

![Step 4 evidence 02](https://github.com/user-attachments/assets/6221092b-c9bb-431e-b561-ca8408d4b50d)

*Evidence:* IP Security Policies on Local Computer is selected, with Create IP Security Policy highlighted in the context menu.

### Step 5

On the welcome page select Next.

*Expected result:* The wizard advances from the welcome page to the policy-name page.
*Verify:* Confirm that the Name and Description fields are available before entering the required policy name.

### Step 6

Name the policy Structureality IPSec Policy (required), then Next.

**Screenshot 03 — Policy name** — `Screenshot 2026-11-19 at 2.07.51 PM.png`

![Step 6 evidence 03](https://github.com/user-attachments/assets/96289580-2f6f-4fd8-9b46-e543fee5a84e)

*Evidence:* Name field shows Structureality IPSec Policy (required).

### Step 7

Leave Requests for Secure Communication unchecked, then Next.

*Expected result:* The wizard advances without creating a default response rule.
*Verify:* Confirm Requests for Secure Communication remains unchecked.

### Step 8

Check Edit properties and select Finish.

*Expected result:* The new policy is created and its Properties window opens.
*Verify:* Confirm Edit properties was checked and the Rules tab is available.

### Step 9

In policy Properties select Add.

*Expected result:* The Create IP Security Rule Wizard opens.
*Verify:* Confirm the wizard is attached to the correct Structureality IPSec Policy.

### Step 10

In Security Rule Wizard select Next.

**Screenshot 04 — Security Rule Wizard** — `Screenshot 2026-11-19 at 2.10.57 PM.png`

![Step 10 evidence 04](https://github.com/user-attachments/assets/cacce8c9-22e8-433a-a1f9-9da71721a927)

*Evidence:* The Create IP Security Rule Wizard welcome page is displayed.

### Step 11

Set This rule does not specify a tunnel and select Next.

**Screenshot 05 — Transport mode** — `Screenshot 2026-11-19 at 2.11.05 PM.png`

![Step 11 evidence 05](https://github.com/user-attachments/assets/5ea8ab57-8b93-4322-a8a1-6fad00417a65)

*Evidence:* This rule does not specify a tunnel is selected.

### Step 12

Set All network connections and select Next.

**Screenshot 06 — All network connections** — `Screenshot 2026-11-19 at 2.11.15 PM.png`

![Step 12 evidence 06](https://github.com/user-attachments/assets/7a03bcfd-563f-4a29-a323-1a57a8f12c34)

*Evidence:* Network Type is All network connections.

**Screenshot 07 — All network connections** — `Screenshot 2026-11-19 at 2.15.06 PM.png`
*Evidence:* Network Type is All network connections.

### Step 13

On IP Filter List select Add.

**Screenshot 08 — Add an IP filter list** — `Screenshot 2026-11-19 at 2.11.42 PM.png`

![Step 13 evidence 07](https://github.com/user-attachments/assets/97f8e6ed-89f0-4b7d-b431-9e3eff7af639)

*Evidence:* The IP Filter List page is displayed with the Add button focused.

### Step 14

Name Structureality IP Filter List and select Add.

**Evidence — Step 14**

![Step 14 evidence 08](https://github.com/user-attachments/assets/3a12398b-d050-42c5-97f6-2e4e0175096b)

**Screenshot 09 — Name the IP filter list** — `Screenshot 2026-11-19 at 2.11.59 PM.png`

*Evidence:* Structureality IP Filter List is entered as the name, and Use Add Wizard is checked.

### Step 15

On IP Filter Wizard welcome select Next.

*Expected result:* The IP Filter Wizard advances to the description and mirrored-property page.
*Verify:* Confirm the new filter is being added to Structureality IP Filter List.

### Step 16

Leave Description blank, ensure Mirrored is checked, then Next.

*Expected result:* The wizard advances to the IP Traffic Source page after accepting a blank description.
*Verify:* Confirm Mirrored is selected so the filter applies bidirectionally.

### Step 17

Set IP Traffic Source to Any IP Address and select Next.

**Screenshot 10 — Any source address** — `Screenshot 2026-11-19 at 2.15.51 PM.png`
*Evidence:* IP Traffic Source = Any IP Address.


### Step 18

Set IP Traffic Destination to Any IP Address and select Next.

**Screenshot 11 — Any destination address** — `Screenshot 2026-11-19 at 2.15.58 PM.png`
*Evidence:* IP Traffic Destination = Any IP Address.


### Step 19

Set IP Protocol Type to Any and select Next.

**Screenshot 12 — Any protocol** — `Screenshot 2026-11-19 at 2.16.10 PM.png`
*Evidence:* IP Protocol Type = Any.


### Step 20

Leave Edit properties unchecked and select Finish.

*Expected result:* The IP Filter Wizard closes and the completed Any-to-Any filter appears in the filter list.
*Verify:* Confirm Edit properties is unchecked and the new filter is listed before continuing.

### Step 21

Confirm IP Filter List and select OK.

*Expected result:* The IP Filter List dialog closes and the Security Rule Wizard displays available filter lists.
*Verify:* Confirm Structureality IP Filter List is present and selectable.

### Step 22

Select Structureality IP Filter List and select Next.

**Screenshot 13 — IP filter list selected** — `Screenshot 2026-11-19 at 2.18.06 PM.png`
*Evidence:* Structureality IP Filter List is selected; two similarly named entries appear.


### Step 23

Ensure Use Add Wizard checked and select Add.

*Expected result:* The Filter Action Wizard opens to create a new action.
*Verify:* Confirm Use Add Wizard is checked before selecting Add.

### Step 24

On Filter Action Wizard welcome select Next.

*Expected result:* The wizard advances to the Filter Action Name page.
*Verify:* Confirm the name field is ready for Encrypt All The Things.

### Step 25

Name filter action Encrypt All The Things and select Next.

**Screenshot 14 — Filter action name** — `Screenshot 2026-11-19 at 2.18.52 PM.png`
*Evidence:* Filter action named Encrypt All The Things.


### Step 26

Select Negotiate security and select Next.

**Screenshot 15 — Negotiate security** — `Screenshot 2026-11-19 at 2.18.42 PM.png`
*Evidence:* Negotiate security selected.


### Step 27

Select Do not allow unsecured communications and select Next.

**Screenshot 16 — No plaintext fallback** — `Screenshot 2026-11-19 at 2.19.09 PM.png`
*Evidence:* Do not allow unsecured communication selected.


### Step 28

Select Integrity and encryption and select Next.

**Screenshot 17 — Encryption and integrity** — `Screenshot 2026-11-19 at 2.19.30 PM.png`
*Evidence:* Integrity and encryption selected.


### Step 29

Leave Edit properties unchecked and select Finish.

*Expected result:* The Filter Action Wizard closes and the new action appears in the available Filter Actions list.
*Verify:* Confirm Edit properties is unchecked and Encrypt All The Things is listed.

### Step 30

Select Encrypt All The Things from Filter Actions and select Next.

**Screenshot 18 — Filter action selected** — `Screenshot 2026-11-19 at 2.21.46 PM.png`
*Evidence:* Encrypt All The Things selected; another similarly named action is visible.


### Step 31

Select preshared key authentication; enter the lab-provided key, then Next. Never publish the key in screenshots.

*Expected result:* Preshared-key authentication is selected and the wizard accepts the lab-provided key.
*Verify:* Confirm the preshared-key option remains selected, but do not display or record the secret in the documentation.

### Step 32

Leave Edit properties unchecked and select Finish.

*Expected result:* The authentication selection is saved and the Security Rule Wizard returns to its completion page.
*Verify:* Confirm Edit properties is unchecked before selecting Finish.

### Step 33

Confirm Structureality IP Filter List rule is checked and select OK.

*Expected result:* The completed security rule appears in policy Properties and is enabled.
*Verify:* Confirm its checkbox is selected and that it references Structureality IP Filter List and Encrypt All The Things.

### Step 34

Confirm Structureality IPSec Policy (required) appears in Local Security Policy.

*Expected result:* Structureality IPSec Policy (required) appears in the Local Security Policy right pane.
*Verify:* Confirm the exact policy name and that its initial assignment state is visible.

### Step 35

Right-click policy and select Assign; then use Score to validate.

*Expected result:* The policy assignment state changes to Yes and the CompTIA Score validation succeeds.
*Verify:* Confirm the correct policy—not a similarly named entry—was assigned and the score reports success.

### Step 36

Right-click policy and select Un-assign to restore baseline for subsequent packet capture.

*Expected result:* The policy assignment state returns to No.
*Verify:* Confirm the policy is unassigned so the environment is restored for the later packet-capture comparison.

### Step 37

Leave Local Security Policy open.

*Expected result:* Local Security Policy remains open with the completed policy visible.
*Verify:* Confirm the console is left ready for the next lab activity.

## Security interpretation

- **Negotiate security** instructs Windows to negotiate IPSec for filter-matching traffic.
- **Do not allow unsecured communications** is the central fail-closed control, rather than falling back to plaintext for incompatible peers.
- **Integrity and encryption** requires confidentiality and integrity for protected traffic.
- **Mirrored Any-to-Any filter** is broad bidirectional scope, subject to platform-specific negotiation traffic and possible exemptions.
- **This rule does not specify a tunnel** means host-to-host IPSec transport mode; it is not an IPSec tunnel-mode gateway policy.
- **Preshared key** is specified by the lab, but real-world deployment generally favors centrally managed strong authentication such as certificates or Kerberos where appropriate.

## Verification and results

- [ ] Confirm actual target VM PC10 vs PC20.
- [ ] Confirm security rule's final checkbox and policy properties.
- [ ] Verify Assigned = Yes after Step 35.
- [ ] Confirm successful Score validation.
- [ ] Verify Assigned returns to No after Step 36.
- [ ] In the next exercise validate the plaintext baseline, reapply the policy, check IPSec security associations, and confirm protected ESP traffic between PC10 and PC20.

**Result:** The policy configuration is documented and illustrated by the genuine supplied screenshots. Authentication setup, assignment, scoring, unassignment, and encrypted packet transmission remain execution checks; verify them using the expected states above. Additional screenshots are optional.

## Portfolio skills demonstrated

Windows security administration; IP filter configuration; bidirectional policy scoping; IPSec negotiation controls; encryption and integrity requirements; least-access posture; configuration validation and evidence documentation.

## Embedded screenshot index — optional reference

| Lab step | Image filename | Recorded setting |
| --- | --- | --- |
| 2 | Screenshot 2026-11-19 at 1.58.22 PM.png | Windows Search shows Local Security Policy as the best match. |
| 4 | Screenshot 2026-11-19 at 1.58.59 PM.png | IP Security Policies on Local Computer is selected, with Create IP Security Policy highlighted in the context menu. |
| 6 | Screenshot 2026-11-19 at 2.07.51 PM.png | Name field shows Structureality IPSec Policy (required). |
| 10 | Screenshot 2026-11-19 at 2.10.57 PM.png | The Create IP Security Rule Wizard welcome page is displayed. |
| 11 | Screenshot 2026-11-19 at 2.11.05 PM.png | This rule does not specify a tunnel is selected. |
| 12 | Screenshot 2026-11-19 at 2.11.15 PM.png | Network Type is All network connections. |
| 12 | Screenshot 2026-11-19 at 2.15.06 PM.png | Network Type is All network connections. |
| 13 | Screenshot 2026-11-19 at 2.11.42 PM.png | The IP Filter List page is displayed with the Add button focused. |
| 14 | Screenshot 2026-11-19 at 2.11.59 PM.png | Structureality IP Filter List is entered as the name, and Use Add Wizard is checked. |
| 17 | Screenshot 2026-11-19 at 2.15.51 PM.png | IP Traffic Source = Any IP Address. |
| 18 | Screenshot 2026-11-19 at 2.15.58 PM.png | IP Traffic Destination = Any IP Address. |
| 19 | Screenshot 2026-11-19 at 2.16.10 PM.png | IP Protocol Type = Any. |
| 22 | Screenshot 2026-11-19 at 2.18.06 PM.png | Structureality IP Filter List is selected; two similarly named entries appear. |
| 25 | Screenshot 2026-11-19 at 2.18.52 PM.png | Filter action named Encrypt All The Things. |
| 26 | Screenshot 2026-11-19 at 2.18.42 PM.png | Negotiate security selected. |
| 27 | Screenshot 2026-11-19 at 2.19.09 PM.png | Do not allow unsecured communication selected. |
| 28 | Screenshot 2026-11-19 at 2.19.30 PM.png | Integrity and encryption selected. |
| 30 | Screenshot 2026-11-19 at 2.21.46 PM.png | Encrypt All The Things selected; another similarly named action is visible. |

## Source

[CompTIA Lab on Demand instructions](https://labclient.labondemand.com/Instructions/678c5e7c-f9eb-461c-a1b7-00e8f8a20b53)

## Consultant addendum — enterprise application

Mandatory transport protection can secure east-west server traffic and sensitive administrative flows without changing the application. A production rollout should use narrow filters, certificate or Kerberos authentication, staged enforcement, monitoring, and an exception process.

```powershell
Get-NetIPsecRule
Get-NetIPsecMainModeSA
Get-NetIPsecQuickModeSA
Get-WinEvent -LogName Security -MaxEvents 200 |
  Where-Object Message -Match 'IPsec|IKE|Security Association'
```

**Employer value:** Demonstrates fail-closed control design, rollout-risk analysis, and validation of confidentiality, integrity, and availability impacts.

