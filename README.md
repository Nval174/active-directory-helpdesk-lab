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

I've built the AWS network, deployed the Windows servers, created the `[lab domain]` Active Directory domain, joined Windows client to the domain, configured workstation Group Policy, and completed three help desk scenarios.

### AWS network

- Region: US East (N. Virginia) (`us-east-1`)
- VPC: `[lab network resource]` — `[redacted address]`
- Initial public subnet: `[lab network resource]` — `[redacted address]`
- Compute subnet: `[lab network resource]` — `[redacted address]`
- Internet Gateway: `[lab network resource]`
- Public route table: `[lab network resource]`
- Default route: `[redacted address]` → Internet Gateway

### Windows / Active Directory

| Host | Role | Status |
|---|---|---|
| directory server | Windows Server / Domain Controller / DNS | Complete |
| Windows client | Domain-joined workstation | Complete |

Domain: `[lab domain]`

The lab currently includes fictional users and groups, a `[workstation policy or OU]` OU, a workstation computer GPO, and a working RDP/domain-authentication workflow.

## Current Architecture

```text
AWS us-east-1
└── [lab network resource]
    │   [redacted address]
    │
    ├── [lab network resource]
    │
    ├── [lab network resource]
    │   └── [redacted address]
    │
    └── [lab network resource]
        └── [redacted address]
            │
            ├── directory server
            │   ├── Active Directory Domain Services
            │   ├── DNS
            │   └── [lab domain]
            │
            └── Windows client
                └── Joined to [lab domain]
```

## What I've Done So Far

I've worked through several real troubleshooting situations instead of only building the environment:

- Launched Windows Server instances and worked around an EC2 Availability Zone/instance-type issue.
- Configured Windows client to use directory server for DNS.
- Troubleshot `nltest /dsgetdc:[lab domain]` returning `ERROR_NO_SUCH_DOMAIN` even though DNS resolution worked.
- Tested the Active Directory connectivity needed between Windows client and directory server and corrected the AWS security-group rules.
- Joined Windows client to `[lab domain]` and verified domain authentication with the lab employee.
- Created `[workstation policy or OU]` and verified that the computer-side GPO applies to Windows client.
- Troubleshot an RDP logon-rights problem caused by the workstation GPO and corrected the role-based access configuration.
- Created and resolved an account-lockout scenario for the lab employee.
- Onboarded the support employee as a Help Desk user and assigned role-based group membership.
- Created and resolved a Help Desk share access problem by troubleshooting NTFS permissions.

## Help Desk Tickets

| Ticket | Scenario | Status |
|---|---|---|
| [#001 — Account Lockout](tickets/001-account-lockout.md) | the lab employee account locked after failed logons | Resolved |
| [#002 — New User Onboarding](tickets/002-new-user-onboarding.md) | Create and provision the support employee | Resolved |
| [#003 — Help Desk Share Access](tickets/003-resource-access.md) | User receives Access Denied to a department resource | Resolved |

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
