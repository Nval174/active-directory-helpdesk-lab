# Build Notes

This document records the lab build process, decisions, configuration changes, problems encountered, and verification steps.

## Build Log

### Initial Repository Setup

- Repository created: `active-directory-helpdesk-lab`
- Purpose: document an independently built Active Directory help desk lab
- Lab deployment: started

### Milestone 1 — AWS VPC Created

- AWS Region: US East (N. Virginia) (`us-east-1`)
- VPC name: `AD-Helpdesk-Lab-VPC`
- IPv4 CIDR: `10.0.0.0/16`
- Purpose: provide the network boundary for the Active Directory lab
- Status: Complete

#### Why this configuration was selected

The `/16` VPC provides a large private IPv4 address space while keeping the lab network simple. A smaller subnet will be carved from this VPC for the lab systems.

### Milestone 2 — Public Subnet Created

- Subnet name: `AD-Helpdesk-Public-Subnet`
- IPv4 CIDR: `10.0.0.0/20`
- VPC: `AD-Helpdesk-Lab-VPC`
- Public IPv4 auto-assignment: enabled for the subnet
- Status: Complete

#### Why this configuration was selected

The `/20` subnet provides more address space than the lab requires, while keeping the network easy to understand and manage. It is contained within the `10.0.0.0/16` VPC address space.

### Milestone 3 — Internet Gateway Created

- Internet Gateway name: `AD-Helpdesk-IGW`
- Attached VPC: `AD-Helpdesk-Lab-VPC`
- Status: Complete

#### Purpose

The Internet Gateway provides a path between the VPC and the public internet. It will eventually allow administrative access to the lab systems when combined with appropriate routing and security controls.

### Milestone 4 — Public Route Table Created

- Route table name: `AD-Helpdesk-Public-RT`
- Associated subnet: `AD-Helpdesk-Public-Subnet`
- Routes:
  - `10.0.0.0/16` → `local`
  - `0.0.0.0/0` → `AD-Helpdesk-IGW`
- Status: Complete

#### Networking concepts learned

The local route allows communication within the VPC address space. The `0.0.0.0/0` route acts as the default IPv4 route for destinations outside the VPC and sends that traffic to the Internet Gateway.

### Current Network Architecture

```text
AWS us-east-1
└── AD-Helpdesk-Lab-VPC
    └── 10.0.0.0/16
        │
        ├── AD-Helpdesk-IGW
        │
        └── AD-Helpdesk-Public-Subnet
            └── 10.0.0.0/20
                │
                └── AD-Helpdesk-Public-RT
                    ├── 10.0.0.0/16 → local
                    └── 0.0.0.0/0 → AD-Helpdesk-IGW
```

## Cost Considerations

The lab is being built with AWS cost minimization as a priority. AWS credits are available for the account, but resources will not be left running unnecessarily. Before launching compute resources, instance type, storage, public IPv4, and other potentially billable resources will be reviewed.

## Next Milestone

The next phase will verify the AWS network configuration and prepare the security controls required before deploying the Windows Server instances.
