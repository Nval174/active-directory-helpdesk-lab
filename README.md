# Active Directory Help Desk Lab

I'm building this lab to get more hands-on experience with Windows administration, Active Directory, networking, and the kinds of troubleshooting a help desk technician would actually run into. I'm also using it to build a stronger foundation for cybersecurity.

This project was inspired by a public Active Directory AWS lab by Zackary R. I'm rebuilding the environment myself, documenting what I learn along the way, and adding my own help desk scenarios and security-focused exercises.

## What I'm Trying to Learn

- How AWS networking works in practice
- Windows Server administration
- Active Directory and DNS
- Creating and managing users, groups, and OUs
- Group Policy
- Common help desk troubleshooting
- PowerShell administration and automation
- Basic identity and access management concepts
- How to document technical work clearly

## Where I'm At

**Current status: Core Active Directory help desk lab is working.**

I've built the AWS network, deployed the Windows servers, created the `corp.local` Active Directory domain, joined CLIENT01 to the domain, configured workstation Group Policy, and completed three help desk scenarios.

### AWS network

- Region: US East (N. Virginia) (`us-east-1`)
- VPC: `AD-Helpdesk-Lab-VPC` — `10.0.0.0/16`
- Initial public subnet: `AD-Helpdesk-Public-Subnet` — `10.0.0.0/20`
- Compute subnet: `AD-Helpdesk-Compute-Subnet` — `10.0.16.0/20`
- Internet Gateway: `AD-Helpdesk-IGW`
- Public route table: `AD-Helpdesk-Public-RT`
- Default route: `0.0.0.0/0` → Internet Gateway

### Windows / Active Directory

| Host | Role | Status |
|---|---|---|
| DC01 | Windows Server / Domain Controller / DNS | Complete |
| CLIENT01 | Domain-joined workstation | Complete |

Domain: `corp.local`

The lab currently includes fictional users and groups, a `Lab-Workstations` OU, a workstation computer GPO, and a working RDP/domain-authentication workflow.

## Current Architecture

```text
AWS us-east-1
└── AD-Helpdesk-Lab-VPC
    │   10.0.0.0/16
    │
    ├── AD-Helpdesk-IGW
    │
    ├── AD-Helpdesk-Public-Subnet
    │   └── 10.0.0.0/20
    │
    └── AD-Helpdesk-Compute-Subnet
        └── 10.0.16.0/20
            │
            ├── DC01
            │   ├── Active Directory Domain Services
            │   ├── DNS
            │   └── corp.local
            │
            └── CLIENT01
                └── Joined to corp.local
```

## What I've Done So Far

I've worked through several real troubleshooting situations instead of only building the environment:

- Launched Windows Server instances and worked around an EC2 Availability Zone/instance-type issue.
- Configured CLIENT01 to use DC01 for DNS.
- Troubleshot `nltest /dsgetdc:corp.local` returning `ERROR_NO_SUCH_DOMAIN` even though DNS resolution worked.
- Tested the Active Directory connectivity needed between CLIENT01 and DC01 and corrected the AWS security-group rules.
- Joined CLIENT01 to `corp.local` and verified domain authentication with John Smith.
- Created `Lab-Workstations-Baseline` and verified that the computer-side GPO applies to CLIENT01.
- Troubleshot an RDP logon-rights problem caused by the workstation GPO and corrected the role-based access configuration.
- Created and resolved an account-lockout scenario for John Smith.
- Onboarded Sophia Martinez as a Help Desk user and assigned role-based group membership.
- Created and resolved a Help Desk share access problem by troubleshooting NTFS permissions.

## Help Desk Tickets

| Ticket | Scenario | Status |
|---|---|---|
| [#001 — Account Lockout](tickets/001-account-lockout.md) | John Smith account locked after failed logons | Resolved |
| [#002 — New User Onboarding](tickets/002-new-user-onboarding.md) | Create and provision Sophia Martinez | Resolved |
| [#003 — Help Desk Share Access](tickets/003-helpdesk-share-access.md) | User receives Access Denied to a department resource | Resolved |

## Keeping Costs Down

I have AWS credits available for this project, but I don't want to burn through them just for a lab. I'm using smaller instance resources where practical, watching for potentially billable resources, and avoiding leaving compute running when I'm not using it.

## Documentation

- [AWS Networking](aws-networking/README.md)
- [Architecture](documentation/architecture.md)
- [Build Notes](documentation/build-notes.md)
- [Lessons Learned](documentation/lessons-learned.md)
- [Evidence Notes](documentation/evidence-notes.md)
- [Help Desk Tickets](tickets/README.md)
- [PowerShell](powershell/README.md)

## Credit to the Original Project

This project was inspired by the publicly available `active-directory-aws-lab` project by Zackary R. The lab I'm building here is my own implementation, configuration, documentation, screenshots, troubleshooting, and future additions.
