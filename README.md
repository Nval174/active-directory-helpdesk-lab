# Active Directory Help Desk Lab

I'm building this lab to get more hands-on experience with Windows administration, Active Directory, networking, and the kinds of troubleshooting a help desk technician would actually run into. I'm also using it to build a stronger foundation for cybersecurity.

This project was inspired by a public Active Directory AWS lab by Zackary R. I'm rebuilding the environment myself, documenting what I learn along the way, and planning to add my own help desk scenarios and security-focused exercises.

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

**Current status: AWS network foundation is complete.**

So far I've built the basic network for the lab in AWS US East (N. Virginia):

- VPC: `[lab network resource]` — `[redacted address]`
- Subnet: `[lab network resource]` — `[redacted address]`
- Internet Gateway: `[lab network resource]`
- Route table: `[lab network resource]`
- Default route: `[redacted address]` → Internet Gateway

I haven't deployed the Windows machines or Active Directory yet. I'm building the environment in stages so I can understand what each part does instead of just following a tutorial and moving on.

## Current Architecture

```text
AWS us-east-1
└── [lab network resource]
    │   [redacted address]
    │
    ├── [lab network resource]
    │
    └── [lab network resource]
        │   [redacted address]
        │
        └── [lab network resource]
            ├── [redacted address] → local
            └── [redacted address] → [lab network resource]
```

## What's Coming Next

The next part of the build is securing the network and preparing it for the Windows Server instances.

Eventually the lab will include:

| Host | Role | Status |
|---|---|---|
| directory server | Windows Server / Domain Controller / DNS | Not deployed |
| Windows client | Windows client / domain-joined workstation | Not deployed |

I'll then use the environment to work through realistic help desk situations such as:

- New user onboarding
- Password resets and account unlocks
- Group and access changes
- Group Policy troubleshooting
- Domain-join problems
- Basic Windows troubleshooting

## Keeping Costs Down

I have AWS credits available, but I don't want to burn through them just for a lab. I'll keep an eye on potentially billable resources, avoid leaving compute running when I'm not using it, and review costs before adding anything significant.

## Documentation

- [AWS Networking](aws-networking/README.md)
- [Architecture](documentation/architecture.md)
- [Build Notes](documentation/build-notes.md)
- [Lessons Learned](documentation/lessons-learned.md)
- [Help Desk Tickets](tickets/README.md)
- [PowerShell](powershell/README.md)

## Credit to the Original Project

This project was inspired by the publicly available `active-directory-aws-lab` project by Zackary R. The lab I'm building here is my own implementation, configuration, documentation, screenshots, and future additions.
