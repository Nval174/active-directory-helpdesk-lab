# Lab Roadmap

Planning update: **October 17–18, 2026**. I keep completed results separate from planned features and omit live connection details from this public document.

## How This Fits the Earlier Plan

My September Linux IT Operations roadmap used phases 0–3 for the foundation, phase 4 for a Python/SQL service, phase 5 for operations and recovery, phase 6 for automation, and phase 7 for final validation and documentation.

I used a help-desk portal for phase 4 instead of the originally proposed health collector. The deployed service uses Flask, SQLite, Gunicorn, and Nginx. This roadmap follows the application I actually built.

| Phase | Current position |
|---|---|
| 0–3: Foundation and directory integration | Implemented; see the build record for the exact checks and access-policy scope |
| 4: Python/SQL application | Internal ticket workflow deployed and tested with fictional data |
| 5: Operations and recovery | Local retention, private S3 backup/restore, configuration-package restoration, and recovered-source application checks passed; replacement-host launch preparation passed; full rebuild remains |
| 5A–5C: Certificates, intranet, and domain sign-in | New planned extension; not deployed |
| 6: Automation | Follow the validated manual configuration |
| 7: Final review and portfolio | Repeat acceptance checks and publish sanitized evidence |

## Finish the Recovery Foundation

Before expanding the web service, I am completing these recovery checkpoints:

- [x] Install and verify a 14-copy local backup policy.
- [x] Store encrypted, versioned backups in private S3 storage and test download, checksum, and database restoration.
- [x] Verify cloud retention rules and preserve the current latest recovery copy from date-based expiration.
- [x] Back up and restore the reviewed application/configuration files and record dependency versions; exclude the live secret and machine credentials.
- [x] Test the recovered application source against a disposable restored database with CSRF protection enabled, then run the original regression tests.
- [x] Prepare a separate recovery role, select exact backup versions, validate the image/network configuration, and pass an EC2 launch permission dry run.
- [ ] Launch the replacement host and install fresh dependencies from the recorded inventory.
- [ ] Generate a fresh application secret and verify the complete Gunicorn/Nginx deployment from the Windows client without changing the live service.
- [ ] Observe a later unattended database backup with the repaired S3 hook loaded.
- [ ] Add useful service and backup-failure monitoring and repeat the acceptance checks before automation.

The local database comparison found one ticket in each copy with no missing or extra rows. S3 readback and database checks passed. All 36 archived files were restored and verified, and the actual recovered source passed 12 recovery checks plus four original regression tests. The live database stayed unchanged.

The replacement host has not been launched. The recovered-source drill reused the existing host's verified Python environment, so it does not yet prove a clean server rebuild or fresh dependency installation. See the [recovery session record](backup-and-recovery.md) for the evidence and remaining boundaries.

## Planned Intranet Extension

I will insert this extension **after phase 5 and before phase 6**. It connects Windows certificate practice to the existing Linux application without treating host domain membership as web authentication.

### 5A — Windows certificates and trusted HTTPS

**Goal:** Open the internal site over HTTPS with a certificate trusted by the lab client.

- Build a small private PKI using Active Directory Certificate Services (AD CS).
- Learn the Windows certificate stores, certificate chain, certificate request, DNS subject alternative names, Server Authentication usage, and private-key permissions.
- Limit certificate-template enrollment to the intended lab role; do not enable broad requestor-supplied identities or unnecessary enrollment services.
- Issue a server certificate for the chosen internal DNS name and install it on the selected web front end.
- Verify client trust and make the required certificate/revocation distribution locations reachable internally.
- Document renewal, revocation, CA backup, and recovery. Keep CA keys and exported key material outside Git.

**Pass:** The intended hostname loads without a trust/name warning from the lab client. A wrong-name or untrusted certificate fails validation. Renewal works. Test revocation with explicit revocation checking and recorded client behavior; do not assume every browser blocks identically.

### 5B — Company intranet homepage

**Goal:** Give fictional employees a useful internal starting page.

- Add a homepage with company announcements, support information, and a clear Help Desk entry.
- Keep the ticket service on Linux; choose between a protected help-desk route or a separate protected internal site.
- Preserve existing tickets and back up before any schema or deployment change.
- Keep the draft homepage private while developing it; the complete intranet is not accepted until phase 5C passes.

**Pass:** Navigation, ticket links, form submissions, redirects, and error pages work under the chosen hostname/path. HTTPS works throughout. No new public web access is introduced.

### 5C — Domain sign-in and ticket permissions

**Goal:** An approved domain user can use their Windows sign-in to enter the intranet and help desk; anonymous and unauthorized identities are denied.

My preferred Windows learning experiment is **IIS Windows Authentication on a domain-member web gateway**, with Kerberos verified in the browser/server session. The gateway can front the existing Linux application. I will validate the proxy, path handling, identity handoff, maintained component support, and protocol support in a small pilot before adopting this design.

- Disable anonymous access to protected content and explicitly authorize the intended AD group. A local Windows account must not satisfy the domain-only policy.
- Configure DNS, the HTTP service identity/SPN, and the browser's internal-site sign-in policy for the selected hostname. Record the protocol actually negotiated; do not call an NTLM fallback a successful Kerberos test.
- Protect the homepage, ticket pages, write operations, and any API/export endpoints with the same policy.
- Restrict backend access to the authenticated gateway. Remove direct client paths that could bypass it.
- Derive identity from the trusted authentication component and reject client-supplied identity headers. Do not put domain administrator credentials in the application or enable unconstrained delegation for this design.
- Add server-side ticket ownership and role checks: employees work with their own tickets, support staff triage permitted tickets, and administration is a separate role.
- Record authenticated identity, authorization outcome, and useful events without logging passwords, authentication tokens, or private keys.

**Pass:** An allowed domain user enters through the Windows browser and gets the intended access. Anonymous requests, a local-only account, an excluded domain user, an employee accessing another employee's ticket, forged identity headers, and direct backend bypass are denied. Repeat new-session tests after removing group access and account for cached tickets/sessions. Verify behavior when the directory service is unavailable.

A trusted HTTPS server certificate authenticates the server and protects transport. It does not by itself prove which Windows user is visiting. Windows sign-in and AD-group authorization provide that user access boundary.

### Resource and Topology Decision Before Building

I plan to keep the certificate authority separate from the domain controller and web gateway, and keep the existing Windows client for testing. A dedicated CA and a dedicated IIS member gateway could add **two Windows instances**. I will check capacity, licensing, and cost before choosing or provisioning them.

If that footprint is too large, I can evaluate a supported Linux Kerberos gateway as an alternative front end. Existing Nginx does not acquire Windows single sign-on merely because the Linux host is domain joined. The implementation choice remains open until the pilot and resource review.

Certificate distribution and revocation checking need their own narrow internal access rules; they must not depend on first authenticating to the protected employee homepage. Any proxy-to-application hop will use a documented, restricted transport boundary; I will decide and validate its TLS requirements during the pilot.

### Optional Certificate Experiments After 5C

I can later add certificate auto-enrollment, renewal troubleshooting, and user/device certificates. Requiring a managed device as well as an approved user is an additional control, such as validated client certificates with appropriate mapping. It is separate from signing into Windows with a domain account.

## Then Automate and Revalidate

For phase 6, automate only the configuration that passed the manual checkpoints. Handle enrollment and secrets explicitly; do not embed credentials in playbooks.

For phase 7, repeat directory, client, application, permission, TLS, backup/restore, and restart checks. Update the architecture diagram only after the new roles are actually deployed, and document controlled failure/recovery scenarios with fictional data.

The private cybersecurity study planner is a separate application. This intranet experiment does not change its audience or monthly learning review.

## References

Microsoft documentation reviewed October 18, 2026:

- [Active Directory Certificate Services overview](https://learn.microsoft.com/en-us/windows-server/identity/ad-cs/active-directory-certificate-services-overview)
- [IIS Windows Authentication](https://learn.microsoft.com/en-us/iis/configuration/system.webserver/security/authentication/windowsauthentication/)
- [IIS URL authorization](https://learn.microsoft.com/en-us/iis/configuration/system.webserver/security/authorization/)
- [Server certificate deployment overview](https://learn.microsoft.com/en-us/windows-server/networking/core-network-guide/cncg/server-certs/server-certificate-deployment-overview)

See the [build record](linux-operations.md), [operations runbook](operations-runbook.md), and [evidence notes](evidence-notes.md) for completed work and its limits.
