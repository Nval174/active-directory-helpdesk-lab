# Lab Roadmap

Recorded build checkpoint: **October 18, 2026**. Plan reviewed **October 22, 2026**.

| Phase | Position |
|---|---|
| 0–3 — Windows/Linux foundation | Working baseline; Linux access restricted to the administration role |
| 4 — Python/SQL service | Help-desk portal deployed in place of the originally proposed health collector |
| 5 — Recovery | Database/file restoration and recovered-source tests passed; replacement-host rebuild pending |
| 5A–5E — Extensions | Certificates, intranet, domain sign-in, VPN, centralized logging; planned |
| 6 — Automation | Pending manual acceptance checks |
| 7 — Portfolio | Documentation/screenshots published; reviewed source and final evidence pending |

## Current work

- [ ] Complete the [replacement-host drill](operations-runbook.md#next-recovery-drill), including fresh dependencies, browser access, restart persistence, and measured rebuild time.
- [ ] Observe an unattended backup with the repaired S3 hook.
- [ ] Test denied web access, repeat role/revocation checks, and review the older broad connectivity allowance.

The [recovery record](backup-and-recovery.md) holds completed results.

## Planned extensions

| Phase | Goal | Acceptance |
|---|---|---|
| **5A — Certificates/HTTPS** | Practice Windows certificate services and protect the internal site with HTTPS | Intended client trusts the correct site certificate; name/trust failures and renewal behavior are tested |
| **5B — Intranet** | Add a fictional company homepage linked to Help Desk | Navigation and forms work through the protected internal path; existing tickets remain intact |
| **5C — Domain sign-in** | Authorize approved AD users and enforce employee/support/admin ticket permissions | Anonymous/excluded users, cross-user ticket access, and direct backend bypass are denied; revocation is tested |
| **5D — VPN/network access** | Require both company-network/VPN reachability and website authorization; VPN login uses an approved domain username/password | Approved remote user connects from a personal PC; invalid/disabled/unauthorized accounts and access outside the permitted network path fail |
| **5E — Centralized logging** | Collect selected Linux, Windows, application, VPN, and network events into a protected searchable view | Known test events arrive with correct source/time; missing senders, retention, forwarding interruptions, and coverage gaps are tested |

I have not deployed these extensions. IIS, OpenVPN Access Server, and Graylog are candidates from planning discussions, not selected or validated implementations. Hosting, compatibility, licensing, and cost need review before provisioning.

Logging depends on enabled audit sources and forwarding. AWS flow data requires a separate integration; the target is documented coverage, not a claim to capture every event.

## Automation and portfolio

- [ ] Automate the validated configuration with Ansible and test repeat runs and a replacement deployment.
- [ ] Publish sanitized application source, tests, deployment templates, and backup helpers.
- [ ] Complete service/backup-failure monitoring and repeat the access, recovery, and restart acceptance checks.
- [ ] Capture the SSSD repair and useful recovery results; update the architecture when new roles are deployed.

Background reading for the proposed Windows components: [AD CS](https://learn.microsoft.com/en-us/windows-server/identity/ad-cs/active-directory-certificate-services-overview) and [IIS Windows Authentication](https://learn.microsoft.com/en-us/iis/configuration/system.webserver/security/authentication/windowsauthentication/).
