# Build Notes

This is my running log for the lab. I'm using it to keep track of what I built, why I chose certain settings, and the troubleshooting that was useful to the project.

## Initial Repository Setup

I created this repository to document my own Active Directory help desk lab. The goal is to build the environment myself instead of just copying a tutorial and moving on.

## Milestone 1 — AWS VPC

**Status: Complete**

- Region: US East (N. Virginia) (`us-east-1`)
- VPC: `AD-Helpdesk-Lab-VPC`
- IPv4 CIDR: `10.0.0.0/16`

### Why I chose this

The VPC is the main network boundary for the lab. A `/16` gives me room to add systems later without making the network unnecessarily complicated.

### Evidence

![VPC configuration](../aws-networking/VPC.png)

## Milestone 2 — Public Subnet and Routing

**Status: Complete**

- Subnet: `AD-Helpdesk-Public-Subnet`
- IPv4 CIDR: `10.0.0.0/20`
- VPC: `AD-Helpdesk-Lab-VPC`
- Public IPv4 auto-assignment: enabled
- Internet Gateway: `AD-Helpdesk-IGW`
- Route table: `AD-Helpdesk-Public-RT`

Routes:

- `10.0.0.0/16` → `local`
- `0.0.0.0/0` → `AD-Helpdesk-IGW`

### What I learned

The subnet divides the VPC address space into a smaller network. The route table determines where traffic goes, while the Internet Gateway provides the VPC's path to the internet.

### Evidence

![Subnet configuration](../aws-networking/Subnet.png)

![Route table configuration](../aws-networking/route-table.png)

## Milestone 3 — Windows Server Instances

**Status: Complete**

I launched the Windows Server infrastructure in the AWS environment and configured the domain controller and client networking needed for the Active Directory lab.

During the EC2 setup, `t3.medium` was not available for the selected environment, so I used `t3.small` to keep the lab within the available resources. I also had to launch the instance in a subnet/AZ combination that supported the selected instance type.

The domain controller was configured with the private IPv4 address `10.0.22.196`.

### Security group note

The DC01 security group uses the CLIENT01 security group as the source for the broad inbound TCP rule used during the lab. This keeps the rule scoped to the lab client rather than exposing it to the internet. I plan to review and tighten the rule set during a later hardening phase.

## Milestone 4 — Active Directory and DNS

**Status: Complete**

I installed and configured Active Directory Domain Services and DNS on DC01 and created the `corp.local` domain.

I also created the organizational structure used by the lab, including the user, group, and workstation OUs. The fictional users currently used in the lab are John Smith, Maria Garcia, and Sophia Martinez.

### CLIENT01 DNS and domain-controller discovery troubleshooting

Before joining CLIENT01 to the domain, DNS and domain-controller discovery did not work correctly.

The first checks showed that CLIENT01 was using the expected DNS server address, and `nslookup` was eventually able to resolve the domain controller. However, the following command initially failed:

```text
nltest /dsgetdc:corp.local
Getting DC name failed: Status = 1355 0x54b ERROR_NO_SUCH_DOMAIN
```

I worked through the problem by checking the DNS service, confirming that the `corp.local` zone existed, verifying that `DC01` resolved through DNS, checking Netlogon, running DNS diagnostics on DC01, and testing the required connectivity from CLIENT01. The AWS security group was not initially allowing all of the Active Directory traffic needed between the client and domain controller. After correcting the rules, the connectivity tests succeeded and `nltest /dsgetdc:corp.local` was able to discover `DC01` and `corp.local`.

This was an important troubleshooting step because it demonstrated that successful DNS name resolution alone does not guarantee that a Windows client can locate a domain controller through the required Active Directory services.

## Milestone 5 — Join CLIENT01 to the Domain

**Status: Complete**

Once DNS and domain-controller discovery were working, I joined CLIENT01 to the `corp.local` domain.

A newly joined computer initially appeared in the default **Computers** container, so I moved `CLIENT01` into the `Lab-Workstations` OU. This was important because the workstation Group Policy created later was linked to that OU.

I configured CLIENT01's Remote Desktop Users group so domain users with the intended RDP rights could access the workstation without automatically becoming local administrators.

### Verification

John Smith was able to log in to CLIENT01 using his domain credentials during the domain-join verification. `whoami` showed `CORP\\jsmith`, and the logon server identified DC01.

## Milestone 6 — Workstation Group Policy

**Status: Complete**

I created a computer-side Group Policy Object named `Lab-Workstations-Baseline` and linked it to the `Lab-Workstations` OU.

The policy enables **Always wait for the network at computer startup and logon** under:

`Computer Configuration → Policies → Administrative Templates → System → Logon`

The first `gpresult` checks did not show the GPO because CLIENT01 was not yet in the OU to which the GPO was linked. After moving CLIENT01 into `Lab-Workstations`, I verified the GPO from an elevated Administrator command prompt using:

```cmd
gpresult /scope computer /r
```

`Lab-Workstations-Baseline` appeared under the applied computer GPOs.

A standard user such as John Smith is not expected to see this computer-side GPO under user policy results. The GPO applies to the computer object, not directly to John's user account.

### RDP troubleshooting through Group Policy

During later testing, the same GPO controlled **Allow log on through Remote Desktop Services** and allowed the `HelpDesk-Technicians` and `IT-Administrators` groups. The Administrator account initially was not in the IT-Administrators role, so RDP was denied even though the account was an administrator. I added the Administrator account to the existing `IT-Administrators` group, refreshed Group Policy, and restored administrative RDP access without weakening the workstation policy.

This reinforced that effective permissions can be controlled by Group Policy and that role-based access can be used without adding users directly to broad policy settings.

## Milestone 7 — Help Desk Ticket #001: Account Lockout

**Status: Complete**

I created a realistic account-lockout scenario using John Smith's domain account.

### Initial configuration

The domain's Account Lockout Policy initially had the lockout threshold set to `0`, meaning accounts would not be locked after failed authentication attempts. For the lab scenario, I configured:

- Account lockout threshold: **5 invalid logon attempts**
- Account lockout duration: **15 minutes**
- Reset account lockout counter after: **15 minutes**

I then simulated John entering an incorrect password enough times to trigger the lockout.

### Investigation and resolution

From the administrator side, I confirmed that John's Active Directory account was locked. I unlocked the account and had John log in again with his correct credentials.

The successful login verified that the account issue had been resolved.

## Milestone 8 — Help Desk Ticket #002: New User Onboarding

**Status: Complete**

I created the `smartinez` domain account for Sophia Martinez, placed the account in the Help Desk user OU, and added her to the `HelpDesk-Technicians` security group.

I verified that Sophia was enabled and that she was not assigned unnecessary administrative groups. During later RDP troubleshooting, she was temporarily given local administrator access on CLIENT01 for testing and that access was removed after the task was complete.

## Milestone 9 — Help Desk Ticket #003: Resource Access

**Status: Complete**

I created a `C:\HelpDeskShare` folder on DC01, shared it as `HelpDeskShare`, and configured read-level access for the `HelpDesk-Technicians` group.

### Troubleshooting

After establishing a working baseline where Sophia could access `\\DC01\HelpDeskShare`, I removed the group's NTFS permission to deliberately simulate an Access Denied ticket.

Sophia could still authenticate and remained a member of `HelpDesk-Technicians`, so I checked the resource permissions instead of changing her account.

The missing NTFS permission was the cause. I restored `CORP\\HelpDesk-Technicians` with Read & Execute, List Folder Contents, and Read permissions.

### Verification

Sophia was able to open `\\DC01\HelpDeskShare` again and read `HelpDesk-Test.txt` successfully.

This ticket reinforced the difference between authentication and authorization and the importance of checking both group membership and resource permissions during troubleshooting.

## Current Environment

```text
AWS us-east-1
└── AD-Helpdesk-Lab-VPC (10.0.0.0/16)
    └── Windows Server / Active Directory
        ├── DC01
        │   ├── Active Directory Domain Services
        │   └── DNS
        │
        └── corp.local
            ├── Lab-Users
            ├── Lab-Groups
            ├── Lab-Workstations
            │   └── CLIENT01
            └── Domain Controllers
                └── DC01
```

## Cost Considerations

I have AWS credits available for this project, but I want to keep the lab as inexpensive as possible. I am using smaller instance resources where practical and will stop or terminate resources when they are no longer needed.

## Next Up

The next part of the lab will build on the Active Directory foundation with additional help desk scenarios, administrative tasks, PowerShell work, and later security hardening.
