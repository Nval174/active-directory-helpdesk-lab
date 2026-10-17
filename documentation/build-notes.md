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

## Latest Session — October 16–17, 2026

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

## Current Checkpoint

The domain foundation, Linux identity integration, host hardening, portal, and manual backup are working. I added the available screenshots to the [screenshot index](../pictures/README.md). I still need a capture of the successful SSSD repair checks.

See [Linux Operations](linux-operations.md) for details and [Operations Runbook](operations-runbook.md) for the next-session checklist.

## Next Up

- Test restoration into a separate database.
- Validate a full instance stop/start cycle.
- Add retention and off-instance backup protection.
- Add monitoring and practical log review.
- Improve application authentication/authorization and transport protection.
- Review older connectivity allowances.
- Capture the successful SSSD repair checks at the next useful checkpoint.
