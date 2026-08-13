# Build Notes

This is my running log for the lab. I'm using it to keep track of what I built, why I chose certain settings, and the troubleshooting that was useful to the project.

## Initial Repository Setup

I created this repository to document my own Active Directory help desk lab. The goal is to build the environment myself instead of just copying a tutorial and moving on.

## Milestone 1 — AWS VPC

**Status: Complete**

- Region: US East (N. Virginia) (`us-east-1`)
- VPC: `[lab network resource]`
- IPv4 CIDR: `[redacted address]`

### Why I chose this

The VPC is the main network boundary for the lab. A `/16` gives me room to add systems later without making the network unnecessarily complicated.

### Evidence

![VPC configuration](../aws-networking/network-vpc.png)

## Milestone 2 — Public Subnet and Routing

**Status: Complete**

- Subnet: `[lab network resource]`
- IPv4 CIDR: `[redacted address]`
- VPC: `[lab network resource]`
- Public IPv4 auto-assignment: enabled
- Internet Gateway: `[lab network resource]`
- Route table: `[lab network resource]`

Routes:

- `[redacted address]` → `local`
- `[redacted address]` → `[lab network resource]`

### What I learned

The subnet divides the VPC address space into a smaller network. The route table determines where traffic goes, while the Internet Gateway provides the VPC's path to the internet.

### Evidence

![Subnet configuration](../aws-networking/network-subnet.png)

![Route table configuration](../aws-networking/network-route-table.png)

## Milestone 3 — Windows Server Instances

**Status: Complete**

I launched the Windows Server infrastructure in the AWS environment and configured the domain controller and client networking needed for the Active Directory lab.

During the EC2 setup, `t3.medium` was not available for the selected environment, so I used `t3.small` to keep the lab within the available resources. I also had to launch the instance in a subnet/AZ combination that supported the selected instance type.

The domain controller was configured with the private IPv4 address `[redacted address]`.

### Security group note

The directory server security group uses the Windows client security group as the source for the broad inbound TCP rule used during the lab. This keeps the rule scoped to the lab client rather than exposing it to the internet.

I plan to review and tighten the rule set during a later hardening phase rather than changing working connectivity in the middle of the build.

## Milestone 4 — Active Directory and DNS

**Status: Complete**

I installed and configured Active Directory Domain Services and DNS on directory server and created the `[lab domain]` domain.

I also created the organizational structure used by the lab, including the user, group, and workstation OUs. The lab user created for testing is **the support employee**, and the domain user used for workstation testing is **the lab employee**.

### Windows client DNS and domain-controller discovery troubleshooting

Before joining Windows client to the domain, DNS and domain-controller discovery did not work correctly.

The first checks showed that Windows client was using the expected DNS server address, and `nslookup` was eventually able to resolve the domain controller. However, the following command initially failed:

```text
nltest /dsgetdc:[lab domain]
Getting DC name failed: Status = 1355 0x54b ERROR_NO_SUCH_DOMAIN
```

I worked through the problem by checking the DNS server, confirming that the `[lab domain]` zone existed, verifying that `directory server` resolved through DNS, checking the DNS configuration on Windows client, and reviewing the directory server security group rules. Some of the initial security group checks showed the required connectivity was not yet working. After correcting the rules, the connectivity checks succeeded and `nltest /dsgetdc:[lab domain]` was able to discover `directory server` and `[lab domain]`.

This was an important troubleshooting step because it demonstrated that successful DNS name resolution alone does not guarantee that a Windows client can locate a domain controller through the required Active Directory services.

### Evidence

Screenshots documenting the DNS and domain-controller troubleshooting are stored in the repository's `pictures/` directory.

## Milestone 5 — Join Windows client to the Domain

**Status: Complete**

Once DNS and domain-controller discovery were working, I joined Windows client to the `[lab domain]` domain.

A newly joined computer initially appeared in the default **Computers** container, so I moved `Windows client` into the `[workstation policy or OU]` OU. This was important because the workstation Group Policy created later was linked to that OU.

I also configured Windows client's **Remote Desktop Users** group so that `[lab domain]\\[employee account]` could access the workstation through RDP without being made a local administrator.

### Verification

the lab employee was able to log in to Windows client using his domain credentials. Verification with `whoami` showed `[lab domain]\\[employee account]`, and the logon server identified directory server.

### Evidence

Screenshots of the domain join, OU placement, and domain-user authentication are stored in `pictures/`.

## Milestone 6 — Workstation Group Policy

**Status: Complete**

I created a computer-side Group Policy Object named `[workstation policy or OU]` and linked it to the `[workstation policy or OU]` OU.

The policy enables **Always wait for the network at computer startup and logon** under:

`Computer Configuration → Policies → Administrative Templates → System → Logon`

The first `gpresult` checks did not show the GPO because Windows client was not yet in the OU to which the GPO was linked. After moving Windows client into `[workstation policy or OU]`, I verified the GPO from an elevated Administrator command prompt using:

```cmd
gpresult /scope computer /r
```

`[workstation policy or OU]` appeared under the applied computer GPOs.

A standard user such as the lab employee is not expected to see this computer-side GPO under user policy results. The GPO applies to the computer object, not directly to the lab employee's user account.

### What I learned

This helped reinforce the difference between **computer policy** and **user policy** in Active Directory. When troubleshooting Group Policy, the computer's OU, the GPO link, and the policy scope all need to be considered.

### Evidence

The successful GPO application and OU/GPO configuration screenshots are stored in `pictures/`.

## Milestone 7 — Help Desk Ticket #001: Account Lockout

**Status: Complete**

I created a realistic account-lockout scenario using the lab employee's domain account.

### Initial configuration

The domain's Account Lockout Policy initially had the lockout threshold set to `0`, meaning accounts would not be locked after failed authentication attempts. For the lab scenario, I configured:

- Account lockout threshold: **a configured threshold**
- Account lockout duration: **a configured interval**
- Reset account lockout counter after: **a configured interval**

I then simulated the lab employee entering an incorrect password enough times to trigger the lockout.

### Investigation and resolution

From the administrator side, I confirmed that the lab employee's Active Directory account was locked. I unlocked the account and had the lab employee log in again with his correct credentials.

The successful login verified that the account issue had been resolved.

### What I learned

The scenario demonstrated a basic help desk workflow: identify whether the problem is an authentication issue, verify the user's account state in Active Directory, make the minimum administrative change necessary, and confirm that the user can authenticate again.

### Evidence

Relevant account-lockout policy, locked-account, and successful-login screenshots are stored in `pictures/`.

## Current Environment

```text
AWS us-east-1
└── [lab network resource] ([redacted address])
    └── Windows Server / Active Directory
        ├── directory server
        │   ├── Active Directory Domain Services
        │   └── DNS
        │
        └── [lab domain]
            ├── [user OU]
            ├── [group OU]
            ├── [workstation policy or OU]
            │   └── Windows client
            └── Domain Controllers
                └── directory server
```

## Cost Considerations

I have AWS credits available for this project, but I want to keep the lab as inexpensive as possible. I am using smaller instance resources where practical and will stop or terminate resources when they are no longer needed.

## Next Up

The next part of the lab will build on the Active Directory foundation with additional help desk scenarios and administrative tasks.