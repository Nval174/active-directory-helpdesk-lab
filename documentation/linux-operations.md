# Linux IT Operations Extension

Build: **October 16–17, 2026**. Recovery follow-up: **October 17–18, 2026**. Live infrastructure and identity details are omitted.

## Foundation and Directory Integration

I revalidated Windows remote access, shared-resource permissions, and role membership before adding Linux. Temporary elevated access was removed, and a live directory query corrected an organizational-unit mismatch in the older notes.

I deployed Ubuntu 24.04 LTS on AWS with encrypted storage and hardened instance metadata access. I updated the system, set persistent naming and directory-aware DNS, checked time synchronization and discovery, and rechecked DNS after reboot.

I created a dedicated directory location for Linux and joined it through realmd/adcli with SSSD. I entered the privileged credential interactively.

| Check | Result |
|---|---|
| Discovery and membership | Directory discovered; Kerberos membership using SSSD recorded |
| Identity service and trust | Service active; join validation passed |
| Directory registration | Computer object in the intended location; name registration resolved |
| Account/group lookup | Lab identities and roles resolved |
| Role restrictions | Intended administrator allowed; excluded employee/support identities denied |
| Elevated commands | Sudo syntax validated; elevated identity check returned `root` |

I enabled home-directory creation and verified an interactive domain-account session. The local key-based administration path remained available. Direct SSH using a domain identity was not part of the verified workflow.

![Directory-account session with identifying output covered](../pictures/linux-ad-login.png)

![Elevated identity check](../pictures/linux-sudo-access.png)

## Host Hardening and SSSD Repair

I disabled root/password SSH login, reduced authentication attempts, and disabled unused forwarding. Configuration syntax passed, and a second key-based session succeeded. The saved capture includes effective SSH settings and the enabled host firewall.

![SSH settings and firewall checks](../pictures/linux-security.png)

The main SSSD service was active while responder units failed. I backed up and validated the configuration, corrected overlapping responder/socket activation for this installation, and rechecked failed units, identity lookup, and machine trust. The follow-up passed.

![Responder failures before repair](../pictures/sssd-error.png)

The successful repair capture is still missing. This image records the failure, not the repair; the correction was specific to this host.

## Internal Help Desk Portal

I deployed an AI-assisted starter application connecting Python and database coursework with Linux operations.

| Layer | Implementation |
|---|---|
| Application | Flask and Flask-WTF; ticket creation, details, updates, and filters |
| Data and validation | SQLite constraints, parameterized SQL, server-side validation, template escaping, and CSRF-protected POST requests |
| Runtime | Gunicorn on loopback behind Nginx |
| Process identity | Dedicated non-login account; administratively owned code and a separate writable data location |
| Configuration | Random server-generated secret in a protected environment file |
| Service | Enabled non-root systemd unit with filesystem, privilege, and temporary-space restrictions |
| Proxy | Request limits and defensive response headers |

The starter passed four tests for health, ticket lifecycle, invalid input, and missing-ticket handling, plus a Gunicorn dashboard/health smoke test. These checks were run on the starter bundle; this repository does not contain an application CI pipeline.

### Deployment and verification

I transferred the bundle from Windows, verified its checksum on Linux, installed dependencies, and initialized SQLite as the application account. A Windows upload command initially failed inside the SSH Bash session; rerunning it from PowerShell corrected the shell mismatch.

I checked the backend listener and service, validated Nginx configuration before reload, and opened the portal from the intended Windows client. I created and updated a fictional ticket.

![Database integrity, service state, and backend health](../pictures/helpdesk-service.png)

![Portal browser workflow](../pictures/helpdesk-portal.png)

A request immediately after a combined proxy/backend restart returned a gateway error. Subsequent direct and proxied health checks succeeded, consistent with a startup-readiness delay. The sample ticket survived that restart and the later instance restart cycle.

The portal remains a restricted prototype. Application login, ticket authorization, and HTTPS are pending; Linux domain membership does not supply web authentication.

## Backups and Recovery

I installed a daily systemd backup timer, then added atomic SQLite backups, an exclusive lock, integrity checks, and retention of the newest 14 completed copies. Private S3 storage uses temporary role credentials, encryption, versioning, verified uploads/readback, and lifecycle rules.

The first observed timer activation exposed a missing cloud upload hook. Its repair passed three complete manual service runs; a later unattended activation remains unobserved.

Database restoration, application/configuration-file restoration, and tests using the actual recovered source passed. The recovery host is prepared but has not been launched. The detailed results and verification limits are maintained in the [backup and recovery record](backup-and-recovery.md).

See the [operations runbook](operations-runbook.md) for session checks and the [roadmap](roadmap.md) for the next milestones.
