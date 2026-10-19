# Build Notes

This is the public, sanitized record of what I built and verified. Operational identifiers and detailed access settings are intentionally omitted.

## Original Windows Foundation

### Network and compute

I created an AWS virtual network, subnets, routing, and internet connectivity. An availability-zone/instance-type limitation required adjusting the deployment choice before the Windows systems could be launched.

### Active Directory and DNS

I installed directory services and DNS, created a lab domain, and organized fictional users and role groups.

The client could eventually resolve the directory server, but domain-controller discovery still failed. I checked DNS, directory health, service discovery, and connectivity rather than assuming that name resolution proved all required traffic was working. Correcting cloud firewall connectivity allowed discovery and domain join to succeed.

### Client policy and remote access

I joined the client, placed its computer object in the intended organizational unit, and verified computer-side Group Policy. I learned that computer policy is not the same as a standard user's policy results.

A remote-access problem was traced to the effective logon-right assignment and role membership. Correcting membership restored intended access without broadening the policy unnecessarily.

### Support scenarios

| Scenario | Work completed |
|---|---|
| Account lockout | Configured a lab policy, triggered a lockout, confirmed the state, unlocked the account, and verified sign-in |
| User onboarding | Created a fictional identity, assigned the required role, and checked that unnecessary privileges were absent |
| Shared-resource access | Isolated an access-denied problem to missing NTFS permissions and restored read-level access |

Temporary elevated access used during troubleshooting was removed.

## Linux Build — October 16–17, 2026

### Foundation revalidation

I rechecked the Windows baseline before extending it. The useful outcomes were confirmed share access, intended allowed/denied remote-access behavior, removal of temporary elevated access, and correction of an organizational-unit mismatch in the documentation.

Detailed setup repetition is omitted; the new work focuses on Linux operations.

### Linux infrastructure and identity

1. Deployed Ubuntu LTS on AWS with encrypted storage and hardened instance metadata settings.
2. Configured persistent naming and directory-aware DNS.
3. Confirmed internal/external resolution and time synchronization.
4. Created a dedicated directory location for Linux and completed domain membership.
5. Validated SSSD identity resolution and machine trust.
6. Restricted domain-account access by role, enabled home creation, and verified role-based sudo.
7. Hardened SSH, retained a tested key-based administration path, and enabled the host firewall.

### Service troubleshooting

The main SSSD process was healthy, but responder units showed failures. I backed up and validated the configuration, corrected a responder/socket-activation conflict, and rechecked failed units, identity resolution, and machine trust.

### Application deployment

I deployed the tested starter portal through a reverse proxy and non-root backend service. The database was initialized under the application identity. Browser access was restricted to the intended lab workflow and a fictional ticket was created and updated.

Immediately after a simultaneous proxy/backend restart, a health request returned a gateway error. Repeating both health paths after startup succeeded, consistent with a brief readiness delay. The sample ticket remained in the database.

### Backups

I installed a scheduled SQLite backup job, manually triggered it, confirmed a copy existed, and checked integrity. Timer enablement/activity and the next-run listing were also verified.

This proves backup creation, not restoration or off-instance recovery.

## Recovery Session — October 17–18, 2026

I restored a local backup into an isolated database and compared full ticket rows: one ticket in each copy, no missing or extra rows. After bringing the three lab instances back online, service, trust, time, database-health, and Windows browser persistence checks passed.

I then added 14-copy local retention and private S3 storage using an instance role. Database uploads/downloads passed encryption, versioning, checksum, exact-byte, integrity, ticket-count, and anonymous-access-denial checks. Cloud lifecycle rules were applied and inspected without replacing unrelated rules.

An observed timer run exposed a missing S3 upload hook. I repaired the loaded service configuration and verified three complete manual backup jobs afterward. I still need to observe a later unattended activation with that hook loaded.

I created a separate application/configuration snapshot with 14 application files and 36 archived files total. The live secret and machine credentials were excluded. Private S3 readback and all restored file hashes passed. Using the actual recovered code and a disposable restored database, 12 recovery checks and four original regression tests passed; live data stayed unchanged.

Finally, I prepared a separate recovery host. Exact image/network checks, a restricted recovery-role policy simulation, and an EC2 launch permission dry run passed. Two backup object versions are pinned for the drill. The host has not been launched, and the full new-host web deployment remains pending.

The [backup and recovery record](backup-and-recovery.md) distinguishes these live results from fixture tests and the remaining rebuild work.

## Current Checkpoint

The domain foundation, Linux identity integration, host hardening, portal, local retention, and private cloud backups are working. Database restoration, configuration-file restoration, and recovered-source application checks passed. A separate recovery host is prepared but not launched. I added the available screenshots to the [screenshot index](../pictures/README.md); the successful SSSD repair capture and new recovery captures remain outstanding.

See [Linux Operations](linux-operations.md) for details and [Operations Runbook](operations-runbook.md) for the next-session checklist.

## Next Up

- Launch the prepared recovery host and verify a complete rebuild with fresh dependencies and browser access.
- Observe a later unattended backup with the repaired cloud hook loaded.
- Add monitoring and practical log review.
- Improve application authentication/authorization and transport protection.
- Review older connectivity allowances.
- Capture the successful SSSD repair checks at the next useful checkpoint.
