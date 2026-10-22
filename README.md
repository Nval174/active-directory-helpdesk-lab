# Active Directory & Linux Help Desk Lab

I built this AWS lab to practice Windows/Linux administration, identity management, application deployment, and help desk troubleshooting.

## Recorded checkpoint — October 18, 2026

| Area | Result |
|---|---|
| Windows | Active Directory, DNS, a domain-joined client, Group Policy, and three help desk exercises |
| Linux | Ubuntu 24.04 LTS joined to AD, restricted administration, SSH hardening, and a host firewall |
| Portal | Flask/SQLite behind Gunicorn and Nginx; ticket workflow, health checks, and restart persistence tested |
| Recovery | Local and private S3 backups restored; recovered application tests passed |

**Next:** rebuild on a separate host. Preparation passed, but that host has not been launched. The [roadmap](documentation/roadmap.md) tracks the remaining work.

![Portal with a fictional sample ticket](pictures/helpdesk-portal.png)

The portal uses fictional data. Application login, ticket permissions, and HTTPS are still pending.

## Project records

- [Architecture](documentation/architecture.md) and [AWS networking](aws-networking/README.md)
- [Build timeline](documentation/build-notes.md) and [Linux configuration](documentation/linux-operations.md)
- [Backup and recovery results](documentation/backup-and-recovery.md) and [operations runbook](documentation/operations-runbook.md)
- [Help desk tickets](tickets/README.md) and [lessons learned](documentation/lessons-learned.md)
- [Screenshots](pictures/README.md) and [evidence notes](documentation/evidence-notes.md)

This repository contains documentation and redacted screenshots. Application source, tests, deployment configuration, and backup helpers are still private; publishing reviewed copies is planned.

## Credit

The Windows foundation was inspired by Zackary R.'s `active-directory-aws-lab`. The portal started from an AI-assisted application; I document my deployment, configuration, tests, and troubleshooting here.
