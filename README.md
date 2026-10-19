# Active Directory & Linux Help Desk Lab

I'm building this AWS homelab to practice Windows and Linux administration, identity management, networking, application deployment, and realistic help desk troubleshooting.

The original Windows foundation was inspired by a public Active Directory AWS lab by Zackary R. This repository documents my implementation, lessons, and extensions.

I use generic labels in this writeup and cover identifying details in screenshots. I keep live connection details and credentials outside the repository.

## Current Status

**The Windows domain, Linux domain membership, and internal portal are working. Local and private cloud backups have passed restoration checks, including tests using the recovered application source.**

Latest build session: **October 16–17, 2026**. Recovery checks and roadmap review: **October 17–18, 2026**.

| Component | Role | Verified result |
|---|---|---|
| Windows domain controller | Directory services and DNS | Domain and DNS checks passed |
| Windows lab client | Domain member and portal browser | Intended access and portal workflow tested |
| Linux lab server | Ubuntu LTS, directory integration, internal application | Identity integration and portal working |
| Backup and recovery | Local SQLite copies, private S3 storage, and an application/configuration package | Database restoration, file restoration, and recovered-source application checks passed |

The new laptop can support smaller local labs, but this project remains on AWS to practice cloud networking and use the existing environment.

## Latest Milestones

- Briefly revalidated the Windows foundation: shared-resource access, role-based remote access, and removal of temporary elevated access.
- Corrected a documentation mismatch after checking the client's actual organizational-unit placement.
- Deployed Ubuntu LTS with encrypted storage and hardened instance settings.
- Configured persistent hostname/DNS settings, checked time synchronization, and joined Linux to Active Directory.
- Restricted domain-account access by role and verified group-based sudo.
- Hardened SSH, retained a tested key-based administration path, and enabled a host firewall.
- Resolved an SSSD responder/socket startup conflict and rechecked identity resolution and machine trust.
- Deployed an internal help-desk portal using Nginx, Gunicorn, Flask, and SQLite under a non-login service account.
- Verified ticket persistence after service restarts and checked both application and proxy health paths.
- Created a daily systemd backup timer and successfully tested backup creation and database integrity.
- Restored a backup into an isolated database: integrity passed, both copies contained one ticket, and comparisons found no missing or extra rows.
- Rechecked services, directory trust, time synchronization, database readiness, and browser ticket persistence after bringing all three instances back online.
- Installed a 14-copy local retention policy and verified successful backup jobs through systemd.
- Added role-based uploads to private, encrypted, versioned S3 storage; verified downloads, checksums, database restoration, anonymous-access denial, and lifecycle rules.
- Found a missing S3 upload hook in the loaded backup service, restored it, and verified three complete manual service runs afterward.
- Preserved 14 application files in a private configuration package and restored and verified all 36 archived files, excluding the live secret and machine credentials.
- Tested the actual recovered application source against an isolated restored database: 12 recovery checks and four original regression tests passed; live database contents stayed unchanged.
- Prepared a separate recovery host with restricted read permissions and two version-pinned backup objects. Launch permission checks passed; the host has not been launched.

See the [Linux build record](documentation/linux-operations.md) and the [backup and recovery session record](documentation/backup-and-recovery.md) for the implementation summary and verification limits.

![The internal help desk portal with a fictional sample ticket](pictures/helpdesk-portal.png)

I created and updated this fictional ticket to check the browser workflow. The blackouts hide lab identifiers; the ticket content is sample data.

## Skills Demonstrated

- AWS network boundaries, routing, and security-group source relationships
- Windows Server, Active Directory, DNS, Group Policy, and NTFS/share permissions
- Linux networking, SSSD/PAM, sudo, SSH, and host firewall management
- Python/Flask, SQLite, virtual environments, and application deployment
- Nginx reverse proxying, Gunicorn, systemd services and timers
- S3 encryption/versioning, instance-role permissions, lifecycle rules, checksums, and isolated recovery drills
- Layered troubleshooting, backup validation, and technical documentation

## Help Desk Scenarios

| Ticket | Scenario | Status |
|---|---|---|
| [001 — Account Lockout](tickets/001-account-lockout.md) | Diagnose and recover a locked lab account | Resolved |
| [002 — New User Onboarding](tickets/002-new-user-onboarding.md) | Create a role-appropriate lab identity | Resolved |
| [003 — Resource Access](tickets/003-resource-access.md) | Resolve a shared-resource permission problem | Resolved |

The portal's sample ticket is fictional application test data, not an additional resolved support scenario in this series.

## Boundaries and Remaining Work

This is a restricted learning prototype using fictional data, not a production-ready ticketing service. Operating-system directory membership does not provide web-application login or single sign-on.

Application authentication/authorization, encrypted web transport, repeatable deployment, and a full replacement-host rebuild remain follow-up work. The recovered-source application checks used the existing host's verified Python environment; installing fresh dependencies and testing the complete proxy/backend deployment on a new host are still pending.

The timer was observed activating the backup service, but that run exposed the missing cloud upload hook. The repaired hook passed subsequent manual service runs; a later unattended run with it loaded has not yet been observed. The application/configuration package is a manual snapshot, separate from the scheduled database backups.

The [roadmap](documentation/roadmap.md) adds a planned Windows certificate, company intranet, and domain sign-in extension after the recovery foundation. These features are not deployed yet.

## Documentation

- [Architecture](documentation/architecture.md)
- [AWS Networking](aws-networking/README.md)
- [Build Notes](documentation/build-notes.md)
- [Linux Operations Build](documentation/linux-operations.md)
- [Backup and Recovery Results](documentation/backup-and-recovery.md)
- [Operations and Next Session](documentation/operations-runbook.md)
- [Roadmap and Planned Intranet](documentation/roadmap.md)
- [Lessons Learned](documentation/lessons-learned.md)
- [Evidence Notes](documentation/evidence-notes.md)
- [Screenshot Index](pictures/README.md)
- [Help Desk Tickets](tickets/README.md)
- [PowerShell](powershell/README.md)

I have added the available milestone screenshots. The SSSD repair capture is still missing; I kept the original failure screenshot and did not relabel it as a successful repair.

## Cost and Session Management

I keep compute resources small and stop instances between sessions. Retained storage and other allocated resources can still incur charges. The next session begins by checking the directory/DNS dependency before bringing dependent workloads back online.

## Publication Scope

I reviewed the current files and all 77 earlier commits, replaced the screenshot versions with redacted copies, and removed identifiers from historical text and commit metadata. This cleanup preserves the commit sequence and original dates. My author address now uses GitHub's no-reply address.

The [evidence notes](documentation/evidence-notes.md) explain the review scope and what still needs to be captured. Historical screenshots record earlier steps; I do not treat them as a current configuration export.

## Credit

The Windows foundation was inspired by Zackary R.'s `active-directory-aws-lab`. The Linux extension is additional project work. The starter portal was prepared with AI assistance and tested before deployment; the hands-on configuration, verification, and troubleshooting are documented here.
