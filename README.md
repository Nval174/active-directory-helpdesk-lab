# Active Directory Help Desk Lab

A hands-on Active Directory and Windows help desk lab built in AWS for learning and portfolio development.

## Project Overview

This project is inspired by a publicly available Active Directory AWS lab and is being independently rebuilt, documented, and expanded to demonstrate practical IT and cybersecurity fundamentals.

The lab will simulate a small business environment with a Windows Server domain controller and a domain-joined Windows client. The project will focus on common help desk and identity-management tasks such as user onboarding, account troubleshooting, Group Policy, permissions, and PowerShell administration.

## Goals

- Build an Active Directory environment in AWS
- Understand AWS networking and Windows Server administration
- Practice common help desk workflows
- Learn user, group, and Organizational Unit management
- Practice Group Policy administration
- Use PowerShell for Windows administration and automation
- Document troubleshooting and ticket-resolution workflows
- Develop evidence-based technical documentation for a cybersecurity portfolio

## Current Progress

**Status: AWS network foundation complete.**

The initial AWS network has been created and documented:

- AWS Region: `us-east-1` (US East — N. Virginia)
- VPC: `AD-Helpdesk-Lab-VPC` (`10.0.0.0/16`)
- Subnet: `AD-Helpdesk-Public-Subnet` (`10.0.0.0/20`)
- Internet Gateway: `AD-Helpdesk-IGW`
- Route table: `AD-Helpdesk-Public-RT`
- Default route: `0.0.0.0/0` → Internet Gateway

Windows Server instances and Active Directory have **not** yet been deployed.

## Current Architecture

```text
AWS us-east-1
└── AD-Helpdesk-Lab-VPC
    │   10.0.0.0/16
    │
    ├── AD-Helpdesk-IGW
    │
    └── AD-Helpdesk-Public-Subnet
        │   10.0.0.0/20
        │
        └── AD-Helpdesk-Public-RT
            ├── 10.0.0.0/16 → local
            └── 0.0.0.0/0 → AD-Helpdesk-IGW
```

## Planned Lab Systems

| Host | Role | Status |
|---|---|---|
| DC01 | Windows Server / Domain Controller / DNS | Not deployed |
| CLIENT01 | Windows client / domain-joined workstation | Not deployed |

## Planned Help Desk Scenarios

- New user onboarding
- Password reset and account unlock
- Group and access management
- Group Policy troubleshooting
- Domain-join troubleshooting
- Basic Windows client troubleshooting

## Documentation

- [Architecture](documentation/architecture.md)
- [Build Notes](documentation/build-notes.md)
- [Lessons Learned](documentation/lessons-learned.md)
- [Help Desk Tickets](tickets/README.md)
- [PowerShell](powershell/README.md)
- [Screenshot Guidelines](screenshots/README.md)

## Cost Management

The lab is being built with cost minimization as a priority. AWS credits are available, but resources will not be left running unnecessarily. Potentially billable resources will be reviewed before deployment and shut down or removed when they are no longer needed.

## Attribution

This project is independently implemented for educational purposes and was inspired by the publicly available `active-directory-aws-lab` project by Zackary R. The implementation, configuration, documentation, screenshots, and future enhancements in this repository are my own work.
