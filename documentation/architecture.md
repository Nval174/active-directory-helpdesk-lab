# Lab Architecture

## Purpose

This document describes the architecture I've built and verified for the Active Directory help desk lab.

## Current Environment

The lab is running in AWS US East (N. Virginia), `us-east-1`.

### Network

| Component | Configuration | Status |
|---|---|---|
| VPC | `AD-Helpdesk-Lab-VPC` / `10.0.0.0/16` | Complete |
| Initial public subnet | `AD-Helpdesk-Public-Subnet` / `10.0.0.0/20` | Complete |
| Compute subnet | `AD-Helpdesk-Compute-Subnet` / `10.0.16.0/20` | Complete |
| Internet Gateway | `AD-Helpdesk-IGW` | Complete |
| Public route table | `AD-Helpdesk-Public-RT` | Complete |

### Current Routing

```text
Destination       Target
10.0.0.0/16       local
0.0.0.0/0         AD-Helpdesk-IGW
```

The compute subnet uses the same route table.

## Current Systems

| Host | Role | Private IP | Status |
|---|---|---|---|
| DC01 | Windows Server / Domain Controller / DNS | `10.0.22.196` | Complete |
| CLIENT01 | Domain-joined workstation | Assigned by AWS | Complete |

Active Directory domain: `corp.local`

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
                └── Domain member
```

## Active Directory Structure

```text
corp.local
├── Lab-Users
│   ├── Employees
│   │   ├── John Smith
│   │   └── Maria Garcia
│   ├── HelpDesk
│   │   └── Sophia Martinez
│   └── IT
├── Lab-Groups
│   ├── Employees
│   ├── HelpDesk-Technicians
│   │   └── Sophia Martinez
│   └── IT-Administrators
│       └── Administrator
├── Lab-Workstations
│   └── CLIENT01
└── Domain Controllers
    └── DC01
```

## Group Policy

I've created a computer-side GPO named `Lab-Workstations-Baseline` and linked it to the `Lab-Workstations` OU. I verified that it applies to CLIENT01 using `gpresult` from an elevated administrative command prompt.

The GPO also controls the **Allow log on through Remote Desktop Services** setting for the workstation. During testing, the setting was limited to `HelpDesk-Technicians` and `IT-Administrators`. The Administrator account initially could not RDP to CLIENT01 because it was not a member of `IT-Administrators`. Adding the account to the existing role group restored the intended administrative RDP access without changing the GPO to a broader configuration.

## Security Boundary

The Internet Gateway provides network connectivity, but security groups determine which inbound traffic is allowed. For the current lab configuration, the DC01 security group uses the CLIENT01 security group as the source for the broad inbound TCP rule used during the Active Directory connectivity troubleshooting. This keeps the rule limited to the lab client instead of exposing it to the internet. RDP access is restricted to my public IP.

I plan to review and tighten the broader TCP rule during a later hardening phase.

## Troubleshooting

Before joining CLIENT01 to the domain, I was able to resolve DC01 through DNS but `nltest /dsgetdc:corp.local` returned `ERROR_NO_SUCH_DOMAIN` (1355). DNS and AD health checks on DC01 passed, while the required client-to-DC connectivity was not initially available. After correcting the security-group rules, the required connectivity tests succeeded and CLIENT01 was able to discover DC01.

A later RDP issue was traced to the workstation GPO's Remote Desktop Services logon-right assignment. The effective policy allowed the Help Desk and IT role groups, so adding the Administrator account to `IT-Administrators` restored administrative RDP access without weakening the policy.

## Help Desk Scenarios

The current lab includes three completed scenarios:

- Account lockout and recovery
- New user onboarding and role-based group assignment
- Help Desk share access denied due to a missing NTFS group permission

## Next

I'll continue expanding the lab with more help desk scenarios, administrative tasks, PowerShell work, and later security hardening.
