# Lab Roadmap

Checkpoint: **October 18, 2026**. This tracks the application I built, using the earlier Linux IT Operations phases. I chose a Flask/SQLite help-desk portal for phase 4 instead of the proposed health collector.

| Phase | Position |
|---|---|
| 0–3: Foundation and directory integration | Implemented with Linux access restricted to the administration role; checks recorded in the [build notes](build-notes.md) and [Linux operations](linux-operations.md) |
| 4: Python/SQL service | Internal ticket workflow deployed and tested with fictional data |
| 5: Operations and recovery | Database/configuration restoration and recovered-source tests passed; full new-host rebuild pending |
| 5A–5C: Certificates, intranet, domain sign-in | Planned extension after recovery; not deployed |
| 6: Automation | Automate the configuration that passes the manual checkpoints |
| 7: Final validation and portfolio | Repeat acceptance checks and publish reviewed source/evidence |

## Finish the Recovery Foundation

- [x] Install and verify the 14-copy local backup policy.
- [x] Verify private, encrypted, versioned S3 backups, database retrieval/restoration, and lifecycle rules.
- [x] Restore the application/configuration package and dependency inventory, excluding the live secret and machine credentials.
- [x] Test actual recovered source with isolated data and CSRF protection, then run the original regression tests.
- [x] Prepare a restricted recovery role, pin backup versions, validate image/network settings, and pass the launch permission dry run.
- [ ] Launch the replacement host, install fresh dependencies, generate a new secret, and rebuild Gunicorn/Nginx.
- [ ] Verify Windows browser access and restart persistence without changing the live service/data; record full rebuild time.
- [ ] Observe an unattended backup with the repaired cloud upload hook loaded.
- [ ] Add service/backup-failure monitoring and practical log review.
- [ ] Test denied web access and review the older broad connectivity allowance.

The replacement host has not been launched. Existing recovered-source tests reused the verified Python environment; they do not prove a clean rebuild. Detailed results and retention rules are in [backup and recovery](backup-and-recovery.md); the [runbook](operations-runbook.md#next-recovery-drill) orders the next drill.

## Planned Intranet Extension

This sits after phase 5 and before automation.

### 5A — Windows certificates and HTTPS

**Goal:** The lab client trusts the internal site's HTTPS certificate.

- Build a private PKI with AD CS; practice certificate stores, chains, requests, DNS subject alternative names, Server Authentication usage, and key permissions.
- Restrict template enrollment to the intended role and avoid broad requestor-supplied identities.
- Install the server certificate on the chosen front end and provide reachable internal certificate/revocation distribution.
- Test renewal/revocation and document CA backup/recovery; keep private keys outside Git.

**Acceptance:** The intended hostname loads without a trust/name warning; wrong-name and untrusted certificates fail. Verify renewal and explicit revocation-check behavior rather than assuming every browser handles revocation identically.

### 5B — Company homepage

**Goal:** Fictional employees have an internal starting page linked to Help Desk.

Add announcements, support information, and navigation while keeping the ticket service on Linux. Choose a protected route or separate internal site, back up before changes, and preserve existing tickets.

**Acceptance:** HTTPS, navigation, forms, redirects, and error pages work under the selected hostname/path. No new public web access is introduced. Access protection must also pass phase 5C.

### 5C — Domain sign-in and ticket permissions

**Goal:** Approved domain users enter through Windows sign-in and receive role-appropriate access.

My preferred experiment is IIS Windows Authentication on a domain-member gateway in front of Linux. A pilot must verify component/proxy support, paths, identity handoff, and Kerberos before adopting it.

- Disable anonymous access and authorize the intended AD group. Configure DNS, service identity/SPN, and browser policy; record the actual protocol rather than counting NTLM fallback as Kerberos.
- Apply access checks to the homepage, ticket pages, write operations, and API/export endpoints.
- Restrict backend access to the gateway and reject client-supplied identity headers. Keep domain administrator credentials out of the application and avoid unconstrained delegation.
- Enforce ticket ownership and separate employee, support, and administrative permissions on the server.
- Log useful identity/authorization events without passwords, tokens, or keys.

**Acceptance:** Allowed users receive intended access. Deny anonymous requests, local-only accounts, excluded domain users, access to another employee's tickets, forged identity headers, and direct backend bypass. Retest after removing group access, account for cached sessions, and check directory-unavailable behavior.

A trusted server certificate protects transport and identifies the server; it does not identify the visiting Windows user.

### Resources and design decisions

I plan to keep the CA separate from the domain controller and gateway, retaining the Windows client for tests. Dedicated CA and IIS roles could add two Windows instances. Capacity, licensing, and cost review must precede provisioning.

A supported Linux Kerberos gateway remains an alternative if that footprint is too large. The pilot will decide the front end and validate proxy-to-backend transport. Certificate/revocation distribution needs narrow access independent of the protected homepage.

Optional later experiments include certificate auto-enrollment, renewal failures, and user/device certificates. Requiring a managed device is a separate control from domain-user sign-in.

## Automation and Publication

After the manual checkpoints, automate the validated build with explicit secret/enrollment handling. Do not embed credentials in playbooks.

Before completing the portfolio:

- [ ] Publish reviewed, sanitized application source, original tests, deployment templates, and backup helpers. They are deployed or privately archived but are not currently versioned in this repository.
- [ ] Repeat directory, client, application, permissions, TLS, backup/restore, and restart checks.
- [ ] Capture the missing SSSD repair evidence and useful recovery milestones.
- [ ] Update the architecture only when new roles are deployed; keep test data fictional.

The private cybersecurity study planner is a separate application.

## References

Microsoft documentation reviewed October 18, 2026:

- [AD CS overview](https://learn.microsoft.com/en-us/windows-server/identity/ad-cs/active-directory-certificate-services-overview)
- [IIS Windows Authentication](https://learn.microsoft.com/en-us/iis/configuration/system.webserver/security/authentication/windowsauthentication/)
- [IIS URL authorization](https://learn.microsoft.com/en-us/iis/configuration/system.webserver/security/authorization/)
- [Server certificate deployment](https://learn.microsoft.com/en-us/windows-server/networking/core-network-guide/cncg/server-certs/server-certificate-deployment-overview)
