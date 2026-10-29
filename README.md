# Active Directory Deployment

**Hands-on Windows Server / Microsoft Azure lab** · Active Directory Domain Services · DNS · Organizational Units · Identity & Access Management · Windows domain join

## Project overview

Deployed an on-premises-style **Active Directory domain inside Azure** using a Windows Server domain controller (`DC-1`) and a Windows 11 workstation (`Client-1`). Installed and configured Active Directory Domain Services (AD DS), created a new forest, administered organizational units and domain administrator membership, and successfully joined the client workstation to the domain.

**Skills demonstrated:** Windows Server administration · AD DS and DNS · domain controller promotion · Active Directory Users and Computers (ADUC) · user and group management · privileged account administration · organizational units · domain join · RDP · identity troubleshooting.

## Lab environment

| Component | Configuration |
| --- | --- |
| Cloud hosting | Microsoft Azure — East US 2 |
| Resource group | `Active-Directory-Lab` |
| Network | `Active-Directory-VNet` / `default` subnet (`10.0.0.0/24`) |
| Domain controller | `DC-1` — Windows Server 2025 Datacenter — private IP `10.0.0.4` |
| Client workstation | `Client-1` — Windows 11 Pro — private IP `10.0.0.5` |
| AD forest / DNS domain | `mydomain.com` (lab-only domain) |
| Admin organizational unit | `_ADMINS` |
| Employee organizational unit | `_EMPLOYEES` |
| Client organizational unit | `_CLIENTS` |
| Privileged lab account | `jane_admin` (Jane Doe) — member of `Domain Admins` |
| Remote administration | Windows App / RDP from macOS |

```text
Azure virtual network: Active-Directory-VNet
          |
    +-----+-------------------------------+
    |                                     |
 DC-1 / 10.0.0.4                    Client-1 / 10.0.0.5
 Windows Server 2025                Windows 11 Pro
 AD DS + DNS                        DNS -> DC-1
 mydomain.com <-------------------- Joined to mydomain.com
    |
    +-- _ADMINS    -> jane_admin (Domain Admins)
    +-- _EMPLOYEES
    +-- _CLIENTS   -> Client-1 computer account
```

## Implemented and verified

1. **Installed AD DS:** Added the Active Directory Domain Services role and management tools, including Group Policy Management and ADUC, on `DC-1`.
2. **Deployed the directory:** Promoted `DC-1` to a domain controller for the new `mydomain.com` forest, with DNS and Global Catalog enabled.
3. **Structured the directory:** Created `_ADMINS` and `_EMPLOYEES` OUs under the new domain.
4. **Managed privileged identity:** Created the `jane_admin` account, added it to the **Domain Admins** security group, and signed into `DC-1` with that account.
5. **Joined a Windows workstation:** Used `Client-1`'s DNS configuration pointing to the domain controller, supplied authorized domain credentials, and received **“Welcome to the mydomain.com domain”**; the client restarted to apply the change.
6. **Verified the computer account:** Located `Client-1` in ADUC, confirmed its domain identity, and moved it into the `_CLIENTS` OU.

### Key validation evidence

**Domain services configured on DC-1**

![Server Manager showing AD DS and DNS services](screenshots/15-dc-1-ad-ds-and-dns-server-manager-roles.png)

**Domain administrator group membership**

![Jane Doe account added to Domain Admins](screenshots/29-confirm-jane-doe-domain-admins-membership.png)

**Successful workstation domain join**

![Windows confirms Client-1 joined mydomain.com](screenshots/40-confirm-client-1-joined-mydomain-com.png)

**Client computer moved into its organizational unit**

![Active Directory Users and Computers showing Client-1 in the _CLIENTS OU](screenshots/48-confirm-client-1-is-in-the-clients-ou.png)

## Screenshot walkthrough

**50 original lab screenshots** are organized by capture time in the [`screenshots/`](screenshots/) directory. The expanded sections below retain the setup, configuration changes, troubleshooting, and final verification evidence.

<details>
<summary><strong>Install Active Directory Domain Services</strong> — screenshots 01–08</summary>

Started with the existing Azure Windows Server VM and installed the AD DS role plus the associated administration tools.

**01. RDP connections to Azure lab VMs**
![RDP connections to Azure lab VMs](screenshots/01-rdp-connections-to-azure-lab-vms.png)

**02. DC-1 Windows Server initial dashboard**
![DC-1 Windows Server initial dashboard](screenshots/02-dc-1-windows-server-initial-dashboard.png)

**03. Server Manager role installation wizard**
![Server Manager role installation wizard](screenshots/03-server-manager-role-installation-wizard.png)

**04. Active Directory Domain Services role selection**
![Active Directory Domain Services role selection](screenshots/04-active-directory-domain-services-role-selection.png)

**05. AD DS prerequisite administration tools**
![AD DS prerequisite administration tools](screenshots/05-ad-ds-prerequisite-administration-tools.png)

**06. Group Policy Management selected**
![Group Policy Management selected](screenshots/06-group-policy-management-selected.png)

**07. Installation confirmation and restart option**
![Installation confirmation and restart option](screenshots/07-installation-confirmation-and-restart-option.png)

**08. AD DS role installation in progress**
![AD DS role installation in progress](screenshots/08-ad-ds-role-installation-in-progress.png)

</details>
<details>
<summary><strong>Create and verify the new domain</strong> — screenshots 09–17</summary>

Promoted `DC-1` into the domain controller for a new `mydomain.com` forest, then verified AD DS and DNS were present. One remote login attempt failed after promotion; the later login uses the new domain administrator account.

**09. Post-deployment domain controller promotion**
![Post-deployment domain controller promotion](screenshots/09-post-deployment-domain-controller-promotion.png)

**10. Create a new forest for mydomain.com**
![Create a new forest for mydomain.com](screenshots/10-create-a-new-forest-for-mydomain-com.png)

**11. Domain controller capabilities and DSRM password**
![Domain controller capabilities and DSRM password](screenshots/11-domain-controller-capabilities-and-dsrm-password.png)

**12. DNS delegation configuration**
![DNS delegation configuration](screenshots/12-dns-delegation-configuration.png)

**13. Domain controller promotion and warnings**
![Domain controller promotion and warnings](screenshots/13-domain-controller-promotion-and-warnings.png)

**14. Post-promotion remote sign-in troubleshooting**
![Post-promotion remote sign-in troubleshooting](screenshots/14-post-promotion-remote-sign-in-troubleshooting.png)

**15. DC-1 AD DS and DNS Server Manager roles**
![DC-1 AD DS and DNS Server Manager roles](screenshots/15-dc-1-ad-ds-and-dns-server-manager-roles.png)

**16. Find Active Directory Users and Computers**
![Find Active Directory Users and Computers](screenshots/16-find-active-directory-users-and-computers.png)

**17. Inspect the mydomain.com domain**
![Inspect the mydomain.com domain](screenshots/17-inspect-the-mydomain-com-domain.png)

</details>
<details>
<summary><strong>Create OUs and a domain administrator</strong> — screenshots 18–29</summary>

Used the GUI in Active Directory Users and Computers to create `_EMPLOYEES` and `_ADMINS`, create the `jane_admin` account, and validate membership in `Domain Admins`.

**18. Open organizational unit creation menu**
![Open organizational unit creation menu](screenshots/18-open-organizational-unit-creation-menu.png)

**19. Create _EMPLOYEES organizational unit**
![Create _EMPLOYEES organizational unit](screenshots/19-create-employees-organizational-unit.png)

**20. Create _ADMINS organizational unit**
![Create _ADMINS organizational unit](screenshots/20-create-admins-organizational-unit.png)

**21. Confirm organizational unit structure**
![Confirm organizational unit structure](screenshots/21-confirm-organizational-unit-structure.png)

**22. Create user within _ADMINS**
![Create user within _ADMINS](screenshots/22-create-user-within-admins.png)

**23. Enter Jane Doe user properties and login name**
![Enter Jane Doe user properties and login name](screenshots/23-enter-jane-doe-user-properties-and-login-name.png)

**24. Configure the Jane Doe lab account password**
![Configure the Jane Doe lab account password](screenshots/24-configure-the-jane-doe-lab-account-password.png)

**25. Review initial Jane Doe group membership**
![Review initial Jane Doe group membership](screenshots/25-review-initial-jane-doe-group-membership.png)

**26. Open Jane Doe group membership management**
![Open Jane Doe group membership management](screenshots/26-open-jane-doe-group-membership-management.png)

**27. Find the Domain Admins security group**
![Find the Domain Admins security group](screenshots/27-find-the-domain-admins-security-group.png)

**28. Validate the Domain Admins group name**
![Validate the Domain Admins group name](screenshots/28-validate-the-domain-admins-group-name.png)

**29. Confirm Jane Doe Domain Admins membership**
![Confirm Jane Doe Domain Admins membership](screenshots/29-confirm-jane-doe-domain-admins-membership.png)

</details>
<details>
<summary><strong>Sign in with domain admin and join Client-1</strong> — screenshots 30–42</summary>

Signed into `DC-1` as the new domain admin, joined the Windows 11 client to `mydomain.com`, and restarted the workstation as required.

**30. Locate the sign-out controls for DC-1**
![Locate the sign-out controls for DC-1](screenshots/30-locate-the-sign-out-controls-for-dc-1.png)

**31. Sign out of the original administrative session**
![Sign out of the original administrative session](screenshots/31-sign-out-of-the-original-administrative-session.png)

**32. Authenticate to DC-1 as jane_admin**
![Authenticate to DC-1 as jane_admin](screenshots/32-authenticate-to-dc-1-as-jane-admin.png)

**33. Confirm Jane Doe session on DC-1**
![Confirm Jane Doe session on DC-1](screenshots/33-confirm-jane-doe-session-on-dc-1.png)

**34. Connect to Client-1 using its local account**
![Connect to Client-1 using its local account](screenshots/34-connect-to-client-1-using-its-local-account.png)

**35. Open the Client-1 system settings**
![Open the Client-1 system settings](screenshots/35-open-the-client-1-system-settings.png)

**36. Locate the Windows domain or workgroup controls**
![Locate the Windows domain or workgroup controls](screenshots/36-locate-the-windows-domain-or-workgroup-controls.png)

**37. Review Client-1 device information**
![Review Client-1 device information](screenshots/37-review-client-1-device-information.png)

**38. Confirm Client-1 initially uses a workgroup**
![Confirm Client-1 initially uses a workgroup](screenshots/38-confirm-client-1-initially-uses-a-workgroup.png)

**39. Submit mydomain.com domain join credentials**
![Submit mydomain.com domain join credentials](screenshots/39-submit-mydomain-com-domain-join-credentials.png)

**40. Confirm Client-1 joined mydomain.com**
![Confirm Client-1 joined mydomain.com](screenshots/40-confirm-client-1-joined-mydomain-com.png)

**41. Accept restart requirement after domain join**
![Accept restart requirement after domain join](screenshots/41-accept-restart-requirement-after-domain-join.png)

**42. RDP disconnect during Client-1 restart**
![RDP disconnect during Client-1 restart](screenshots/42-rdp-disconnect-during-client-1-restart.png)

</details>
<details>
<summary><strong>Verify Client-1 and organize computer objects</strong> — screenshots 43–50</summary>

Confirmed the domain-joined computer object and created `_CLIENTS`, then moved `Client-1` into that OU.

**43. Reopen ADUC to inspect domain computers**
![Reopen ADUC to inspect domain computers](screenshots/43-reopen-aduc-to-inspect-domain-computers.png)

**44. Verify Client-1 computer object and DNS identity**
![Verify Client-1 computer object and DNS identity](screenshots/44-verify-client-1-computer-object-and-dns-identity.png)

**45. Check Client-1 Domain Computers membership**
![Check Client-1 Domain Computers membership](screenshots/45-check-client-1-domain-computers-membership.png)

**46. Create _CLIENTS organizational unit**
![Create _CLIENTS organizational unit](screenshots/46-create-clients-organizational-unit.png)

**47. Prepare to move Client-1 into _CLIENTS**
![Prepare to move Client-1 into _CLIENTS](screenshots/47-prepare-to-move-client-1-into-clients.png)

**48. Confirm Client-1 is in the _CLIENTS OU**
![Confirm Client-1 is in the _CLIENTS OU](screenshots/48-confirm-client-1-is-in-the-clients-ou.png)

**49. Refresh the Active Directory domain view**
![Refresh the Active Directory domain view](screenshots/49-refresh-the-active-directory-domain-view.png)

**50. Final Active Directory OU and container overview**
![Final Active Directory OU and container overview](screenshots/50-final-active-directory-ou-and-container-overview.png)

</details>
## Lessons and security considerations

- **Troubleshooting:** Domain controller promotion generated installation warnings about adapter IP configuration and DNS delegation. The lab proceeded to a working domain; in production, DNS configuration and any delegation requirements must be evaluated for the actual environment.
- **Privileged access:** The administrator account was created and granted `Domain Admins` privileges for lab learning. Apply least privilege, strong password policies, MFA where supported, and separate day-to-day accounts in production. The account's lab-only **password-never-expires** setting is not a production recommendation.
- **Remote access:** RDP in an internet-accessible cloud lab requires strict source filtering or safer options such as Azure Bastion / just-in-time access in real deployments.
- **Resource lifecycle:** `DC-1` and `Client-1` were **retained** for later exercises. Stop/deallocate unused Azure VMs to control cost.

## Follow-on lab

**Creating Users with PowerShell** is a **separate planned project** covering scripted user provisioning and non-administrator Remote Desktop access. This repository documents **Active Directory Deployment (Part 1)** only.
