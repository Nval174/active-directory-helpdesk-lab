# Build Notes

This is my build sequence and the main troubleshooting outcomes. Live operational details are omitted.

## Original Windows Foundation

### Network and directory

I created an AWS virtual network, subnet allocations, routing, and internet connectivity. An availability-zone/instance-type limitation required adjusting the deployment choice.

I installed Active Directory and DNS, created a lab domain, and organized fictional users and role groups. The client could resolve the directory server while domain discovery still failed. Checking DNS, directory health, discovery, and connectivity isolated the problem to cloud firewall access. Correcting that access allowed domain join.

### Policy and remote access

I joined the client, checked its computer object placement, and verified computer-side Group Policy. A remote-access problem required checking both effective logon rights and role membership. Correcting membership restored intended access.

| Support exercise | Verified outcome |
|---|---|
| [Account lockout](../tickets/001-account-lockout.md) | Generated a lockout, confirmed it, unlocked the account, and tested sign-in |
| [User onboarding](../tickets/002-new-user-onboarding.md) | Created the identity and checked role membership and unnecessary privileges |
| [Resource access](../tickets/003-resource-access.md) | Restored missing NTFS read permissions and retested access |

Temporary elevated access used for troubleshooting was removed during later revalidation.

## Linux Extension — October 16–17, 2026

Before adding Linux, I rechecked Windows remote access, share access, and role membership. A live directory query exposed an organizational-unit mismatch in the older notes, which I corrected.

I then:

1. Deployed Ubuntu 24.04 LTS with encrypted storage and hardened instance metadata settings.
2. Configured persistent naming/DNS, checked resolution and time synchronization, and joined Active Directory.
3. Verified identity resolution and machine trust, restricted Linux access by role, and tested sudo.
4. Hardened SSH, tested a second key-based session, and enabled the host firewall.
5. Repaired an SSSD responder/socket conflict and rechecked identities, trust, and failed units.
6. Deployed Flask/SQLite through Gunicorn and Nginx under a dedicated service account.
7. Tested the portal from Windows, created and updated a fictional ticket, and checked restart persistence.
8. Installed and manually tested the scheduled SQLite backup job.

A health request immediately after restarting the proxy and backend returned a gateway error. Both health paths succeeded after startup; the result was consistent with a readiness delay.

See [Linux operations](linux-operations.md) for the implementation and checks.

## Recovery Session — October 17–18, 2026

I restored and compared an isolated database, then rechecked services, trust, time, health, and Windows browser persistence after the three-instance restart cycle.

I added local retention and private S3 backups, verified cloud retrieval and restoration, and configured lifecycle rules. An observed timer run exposed a missing upload hook; the repair passed three complete manual service runs.

I also restored an application/configuration package and tested the actual recovered sources against isolated data. Finally, I prepared a separate recovery host and passed its launch permission dry run without launching it.

The [recovery record](backup-and-recovery.md) contains the counts, policies, test settings, and remaining limits. The full replacement-host rebuild and a later unattended backup after the hook repair remain open.

## Publication

I added the available milestone captures, redacted identifying material in the repository/history, and corrected commit identity to use GitHub's no-reply address. The [evidence notes](evidence-notes.md) list the missing captures; the [roadmap](roadmap.md) tracks remaining implementation work.
