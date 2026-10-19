# Active Directory & Linux Help Desk Lab

I built this AWS homelab to practice Windows and Linux administration, identity management, networking, Python/SQL application deployment, and help desk troubleshooting.

## Status — October 18, 2026

| Area | Completed and verified |
|---|---|
| Windows foundation | Active Directory, DNS, domain-joined client, Group Policy, role-based remote access, and three support scenarios |
| Linux server | Ubuntu 24.04 LTS, persistent DNS, directory membership, role-restricted login/sudo, SSH hardening, and host firewall |
| Internal portal | Flask/SQLite behind Gunicorn and Nginx; ticket workflow, health checks, and persistence across restarts |
| Backup and recovery | Local retention, private S3 storage, database restoration, configuration-file restoration, and tests using recovered application source |
| Replacement host | Image/network checks, restricted recovery role, pinned backup versions, and launch permission dry run passed; not launched |

The full replacement-host rebuild, an unattended backup after the upload-hook repair, monitoring, and negative web-access tests remain pending. Certificates, an intranet homepage, and domain web sign-in are planned extensions.

This is a lab prototype with fictional data. The portal does not yet have application login, ticket authorization, or HTTPS; host domain membership does not provide web single sign-on.

![The internal help desk portal with a fictional sample ticket](pictures/helpdesk-portal.png)

I created and updated this ticket to test the browser workflow. It is separate from the three resolved support exercises below.

## Help Desk Scenarios

| Ticket | Result |
|---|---|
| [001 — Account Lockout](tickets/001-account-lockout.md) | Triggered a lab lockout, confirmed the state, unlocked the account, and verified sign-in |
| [002 — New User Onboarding](tickets/002-new-user-onboarding.md) | Created a fictional identity and verified role-appropriate membership |
| [003 — Resource Access](tickets/003-resource-access.md) | Restored missing NTFS read permissions and verified access |

## Project Records

| Record | Purpose |
|---|---|
| [Build notes](documentation/build-notes.md) | Windows foundation, Linux extension, and session sequence |
| [Architecture](documentation/architecture.md) | Components, service flow, and implemented controls |
| [Linux operations](documentation/linux-operations.md) | Directory integration, hardening, portal deployment, and troubleshooting |
| [Backup and recovery](documentation/backup-and-recovery.md) | Detailed restore results, retention policies, and verification limits |
| [Operations runbook](documentation/operations-runbook.md) | Stop/start checks, troubleshooting, and the next recovery drill |
| [Roadmap](documentation/roadmap.md) | Remaining recovery work, planned intranet, automation, and acceptance checks |
| [AWS networking](aws-networking/README.md) / [networking notes](documentation/networking-notes.md) | Network boundaries and diagnostic method |
| [Lessons learned](documentation/lessons-learned.md) | Technical decisions and troubleshooting lessons |
| [Evidence notes](documentation/evidence-notes.md) / [screenshots](pictures/README.md) | Capture index, redaction scope, and missing evidence |

## Repository Scope

This repository currently contains documentation and redacted screenshots. The deployed application, original tests, service/proxy configuration, and backup helpers are not yet versioned here; publishing reviewed, sanitized source is on the roadmap. Private backups are kept separately.

I use generic labels and omit live connection details, credentials, and identifying screenshot content. The [evidence notes](documentation/evidence-notes.md) describe the history cleanup and review limits.

I keep compute small and stop instances between sessions. Retained storage and other allocated resources can still incur charges.

## Credit

The Windows foundation was inspired by Zackary R.'s `active-directory-aws-lab`. I extended it with Linux and the portal. The starter application was prepared with AI assistance and tested before deployment; these records describe my configuration, verification, and troubleshooting.
