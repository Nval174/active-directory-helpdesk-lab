# Lab Architecture

## Purpose

This document describes the architecture of the Active Directory help desk lab as it is built and verified.

## Current Environment

The lab is being built in AWS US East (N. Virginia), `us-east-1`.

### Network

| Component | Configuration | Status |
|---|---|---|
| VPC | `AD-Helpdesk-Lab-VPC` / `10.0.0.0/16` | Complete |
| Internet Gateway | `AD-Helpdesk-IGW` | Complete |
| Subnet | `AD-Helpdesk-Public-Subnet` / `10.0.0.0/20` | Complete |
| Route Table | `AD-Helpdesk-Public-RT` | Complete |

### Current Routing

```text
Destination       Target
10.0.0.0/16       local
0.0.0.0/0         AD-Helpdesk-IGW
```

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

## Planned Systems

The following systems have not yet been deployed:

| Host | Planned Role | Status |
|---|---|---|
| DC01 | Windows Server / Domain Controller / DNS | Not deployed |
| CLIENT01 | Windows client / domain-joined workstation | Not deployed |

## Planned Active Directory Services

After the AWS network is verified, the lab will be expanded to include:

- Active Directory Domain Services (AD DS)
- DNS
- Domain users and groups
- Organizational Units (OUs)
- Group Policy
- Windows client domain joining
- Help desk troubleshooting scenarios
- PowerShell administration

## Security Boundary

The Internet Gateway provides network connectivity but does not itself determine which inbound traffic is permitted to the Windows systems. Security groups will be configured before the EC2 instances are deployed so that administrative and required application traffic is deliberately restricted.

## Future Documentation

An original architecture diagram and final system addressing information will be added after the environment is deployed and verified.
