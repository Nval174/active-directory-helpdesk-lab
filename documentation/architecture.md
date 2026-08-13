# Lab Architecture

## Purpose

This document describes the architecture I've built and verified for the Active Directory help desk lab.

## Current Environment

The lab is running in AWS US East (N. Virginia), `us-east-1`.

### Network

| Component | Configuration | Status |
|---|---|---|
| VPC | `[lab network resource]` / `[redacted address]` | Complete |
| Initial public subnet | `[lab network resource]` / `[redacted address]` | Complete |
| Compute subnet | `[lab network resource]` / `[redacted address]` | Complete |
| Internet Gateway | `[lab network resource]` | Complete |
| Public route table | `[lab network resource]` | Complete |

### Current Routing

```text
Destination       Target
[redacted address]       local
[redacted address]         [lab network resource]
```

The compute subnet uses the same route table.

## Current Systems

| Host | Role | Private IP | Status |
|---|---|---|---|
| directory server | Windows Server / Domain Controller / DNS | `[redacted address]` | Complete |
| Windows client | Domain-joined workstation | Assigned by AWS | Complete |

Active Directory domain: `[lab domain]`

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
                └── Domain member
```

## Active Directory Structure

```text
[lab domain]
├── [user OU]
│   ├── Employees
│   │   ├── the lab employee
│   │   └── another lab employee
│   ├── HelpDesk
│   └── IT
│       └── the support employee
├── [group OU]
│   ├── Employees
│   ├── [support role]
│   │   └── the support employee
│   └── [IT role]
├── [workstation policy or OU]
│   └── Windows client
└── Domain Controllers
    └── directory server
```

## Group Policy

I've created a computer-side GPO named `[workstation policy or OU]` and linked it to the `[workstation policy or OU]` OU. I verified that it applies to Windows client using `gpresult` from an elevated administrative command prompt.

## Security Boundary

The Internet Gateway provides network connectivity, but security groups determine which inbound traffic is allowed. For the current lab configuration, the directory server security group uses the Windows client security group as the source for the broad TCP rule used to establish Active Directory connectivity. This keeps the rule limited to the lab client instead of exposing it to the internet.

RDP access is restricted to my public IP.

I plan to review and tighten the broader TCP rule during a later hardening phase.

## Troubleshooting

Before joining Windows client to the domain, I was able to resolve directory server through DNS but `nltest /dsgetdc:[lab domain]` returned `ERROR_NO_SUCH_DOMAIN` (1355). DNS and AD health checks on directory server passed, while the required client-to-DC connectivity was not initially available. After correcting the security-group rules, the required connectivity tests succeeded and Windows client was able to discover directory server.

## Next

I'll continue expanding the lab with more help desk scenarios, administrative tasks, PowerShell work, and later security hardening.
