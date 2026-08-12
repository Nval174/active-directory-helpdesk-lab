# Build Notes

This is my running log for the lab. I'm using it to keep track of what I built, why I chose certain settings, and anything I learn or have to troubleshoot along the way.

## Initial Repository Setup

I created this repository to document my own Active Directory help desk lab. The goal is to build the environment myself instead of just copying a tutorial and moving on.

## Milestone 1 — AWS VPC

**Status: Complete**

- Region: US East (N. Virginia) (`us-east-1`)
- VPC: `AD-Helpdesk-Lab-VPC`
- IPv4 CIDR: `10.0.0.0/16`

### Why I chose this

The VPC is the main network boundary for the lab. A `/16` gives me plenty of room to add systems later without making the network complicated.

### Evidence

> **Image placeholder — VPC screenshot**
>
> Place the redacted VPC screenshot here as `../aws-networking/vpc.png`.

## Milestone 2 — Public Subnet

**Status: Complete**

- Subnet: `AD-Helpdesk-Public-Subnet`
- IPv4 CIDR: `10.0.0.0/20`
- VPC: `AD-Helpdesk-Lab-VPC`
- Public IPv4 auto-assignment: enabled

### What I learned

A subnet is a smaller section of the VPC's address space. The `/20` subnet sits inside the `/16` VPC and gives the lab more addresses than it currently needs.

### Evidence

> **Image placeholder — Subnet screenshot**
>
> Place the redacted subnet screenshot here as `../aws-networking/subnet.png`.

## Milestone 3 — Internet Gateway

**Status: Complete**

- Internet Gateway: `AD-Helpdesk-IGW`
- Attached to: `AD-Helpdesk-Lab-VPC`

### What I learned

The Internet Gateway provides the connection between the VPC and the internet. Creating one doesn't automatically mean everything in the VPC is exposed; routing and security rules still control how traffic gets through.

## Milestone 4 — Public Route Table

**Status: Complete**

- Route table: `AD-Helpdesk-Public-RT`
- Associated subnet: `AD-Helpdesk-Public-Subnet`

Routes:

- `10.0.0.0/16` → `local`
- `0.0.0.0/0` → `AD-Helpdesk-IGW`

### What I learned

The local route lets resources in the VPC communicate with each other. The `0.0.0.0/0` route is the default route for traffic going outside the VPC and sends it to the Internet Gateway.

### Evidence

> **Image placeholder — Route table screenshot**
>
> Place the redacted route table screenshot here as `../aws-networking/route-table.png`.

## Current Network

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

I have AWS credits available for this project, but I want to keep the lab as inexpensive as possible. Before adding compute or other potentially billable resources, I'll check what we're using and why. I also don't plan to leave resources running when I'm finished working with them.

## Next Up

The next part of the build is to verify the network and set up the security controls we'll need before launching the Windows Server machines.
