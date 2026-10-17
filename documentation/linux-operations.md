# Linux IT Operations Extension

Build session: **October 16–17, 2026**. This public record explains methods and verified outcomes without publishing live infrastructure or identity details.

## Phase 0 — Brief Foundation Check

Before adding Linux, I revalidated existing Windows remote access, shared-resource permissions, and role membership. Temporary elevated access was removed. A live directory query also exposed a mismatch between the client's actual organizational-unit placement and the older documentation.

Only these meaningful outcomes are carried forward here; the Windows setup is not repeated.

## Linux Infrastructure

I kept the Linux build on AWS to use the existing lab environment and practice cloud connectivity. The deployment used Ubuntu LTS, encrypted storage, restricted administration, and hardened instance metadata access.

I then:

- Updated the operating system and checked time synchronization.
- Set persistent host identity and directory-aware DNS configuration.
- Verified directory-service discovery and external name resolution.
- Rechecked DNS behavior after reboot.

Live addressing, connection targets, launch/resource identifiers, key names, and detailed network rules are not included in this public write-up.

## Active Directory Integration

A dedicated organizational unit was created for the Linux system and protected from accidental deletion. I joined the server using realmd/adcli with SSSD, entering the privileged credential interactively rather than storing it in a script.

| Validation | Result |
|---|---|
| Domain discovery | Directory discovered |
| Membership listing | Configured as a Kerberos member using SSSD |
| Identity service | Active |
| Machine trust | Join validation passed |
| Directory computer object | Present in the intended location |
| Name resolution | Server registration resolved correctly |
| Identity/group lookup | Fictional lab identities and roles resolved |

I restricted domain-account access to the designated administration role, enabled automatic home-directory creation, and verified an interactive domain-account session.

A sudoers rule delegated elevated commands through the same role group. Its syntax was checked before testing, and the elevated identity check succeeded.

The local key-based administration path remained available. Direct SSH login using a directory identity was not part of the verified workflow, and OS membership did not add web-application single sign-on.

![Directory-account session with identifying output covered](../pictures/linux-ad-login.png)

This captures the session and identity-check commands. I covered the account, group, and home-directory output rather than publishing those identifiers.

![The elevated identity check returned root](../pictures/linux-sudo-access.png)

## Host Hardening

I disabled root/password-based SSH access, retained public-key access, reduced authentication attempts, and disabled unused forwarding. Configuration syntax was checked before reload, and a second key-based connection succeeded.

The host firewall was enabled with restrictive incoming defaults and explicit lab allowances. Cloud and host firewall rules were treated as separate layers.

![SSH and firewall checks with identifying details covered](../pictures/linux-security.png)

The image includes the initial firewall state and the final enabled state after configuration.

### SSSD responder repair

Installing the application prerequisites revealed failed SSSD responder services/sockets even though the main identity service and machine trust were working.

The host had overlapping explicit responder configuration and socket activation. I backed up the configuration, removed the conflict for this installation, preserved restrictive ownership/permissions, validated the configuration, and restarted the service.

The follow-up checks showed no failed units, working directory lookups, and a successful trust check. Clearing failed-state indicators alone would not have proved the repair.

This was a host-specific correction, not a universal recommendation to remove responder settings from every SSSD deployment.

![Failed responder units before the SSSD repair](../pictures/sssd-error.png)

This is the failure I investigated. The successful follow-up checks were confirmed during the session, but their screenshot was not in the supplied folder. I have reserved `pictures/sssd-fix.png` for a later capture.

## Internal Help Desk Portal

I deployed an AI-assisted starter application to connect my Linux administration practice with Python and database coursework.

### Stack and capabilities

- Flask application and Flask-WTF form protection
- SQLite ticket storage
- Gunicorn backend behind Nginx
- Ticket creation, details, status/assignee updates, and filters
- Parameterized SQL and server-side input validation
- Database constraints, template escaping, and CSRF-protected POST requests
- A database-reachability health endpoint

Before deployment, the starter passed four tests covering health, a ticket lifecycle, invalid input, and a missing-ticket response. A separate Gunicorn smoke test checked the dashboard and health response. These were starter-bundle checks, not a complete security assessment or an existing CI pipeline.

### Service design

| Control | Implemented approach |
|---|---|
| Process identity | Dedicated system account with a non-login shell |
| Application code | Administratively owned; not writable by the app account |
| Database | Dedicated writable application-data location |
| Configuration | Random server-generated secret in a protected environment file |
| Backend listener | Loopback only |
| Process management | Enabled systemd service |
| Service restrictions | Non-root execution, restricted filesystem writes, private temporary space, and privilege restrictions |
| Front end | Reverse proxy with request limits and defensive response headers |

No live secret, private key, database, access rule export, or executable connection command is included here.

### Deployment verification

I transferred the starter bundle from Windows, checked its checksum on Linux, installed the code and dependencies, and initialized SQLite using the application identity. Database integrity validation succeeded.

A practical correction was recognizing the shell boundary: Windows PowerShell syntax does not work inside an SSH Bash session. I reran the upload from the correct shell.

The application service was enabled and verified, and the backend listener was checked before configuring the proxy.

![Database checks, active service, and successful application health response](../pictures/helpdesk-service.png)

I tested proxy configuration before reloading it, applied restricted lab access, and opened the portal from the Windows client. I created and updated a fictional ticket to check the workflow.

![The portal with a fictional ticket and updated status](../pictures/helpdesk-portal.png)

### Restart and gateway-error investigation

A request immediately following a combined proxy/backend restart returned a gateway error, even though both processes reported active. Subsequent direct-backend and proxied health requests succeeded. This is consistent with a brief startup-readiness race rather than a persistent proxy problem; no configuration change was needed.

A database query also confirmed that the sample ticket survived the restart. I recorded the failure and successful follow-up instead of treating process status alone as proof of readiness.

### Scope limitations

The portal remains a restricted lab prototype with fictional data. Application login, role-based ticket authorization, and encrypted web transport are not yet implemented. Directory membership on the host does not supply those application features.

## Scheduled SQLite Backups

The backup job uses SQLite's backup mechanism instead of a plain copy of an active database. It creates timestamped copies and checks structural integrity before reporting success.

| Item | Verified state |
|---|---|
| Backup script | Administratively owned with restrictive permissions |
| Backup service | Manual activation succeeded |
| Timer | Enabled, active, and listed with a next run |
| Produced copy | Present and integrity-checked |
| Schedule handling | UTC-based with a randomized delay and persistent catch-up behavior |
| Restoration | Not yet tested |

Exact backup paths and timing are omitted from this public record.

Important limitations:

- Copies are local to the same instance/storage, not off-instance disaster recovery.
- A stopped instance cannot execute backups; a persistent timer can catch up after startup.
- UTC schedules do not remain at the same local clock time across daylight-saving changes.
- Retention deletion is not configured yet.
- Database copies do not include every host configuration item or the application secret.
- Integrity checking does not prove useful records can be restored.

## Next Milestone

The next task is an isolated restore: recover a copy into a separate file and compare ticket contents without replacing the live database.

See [Operations Runbook](operations-runbook.md) and [Evidence Notes](evidence-notes.md).
