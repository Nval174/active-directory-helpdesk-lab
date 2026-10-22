# Step-by-Step Lab Overview

I built this lab to practice Windows and Linux administration, help desk support, and running a small application. This page follows the build from the initial AWS setup to the current recovery checkpoint, with a short explanation of what I did and why.

Build checkpoint: **October 18, 2026**. Overview updated: **October 22, 2026**.

1. **Created the AWS network.** I set up the virtual network, subnets, routing, and cloud firewall rules. This gave the lab machines a shared environment with controlled access.

2. **Built the Windows domain.** I configured a Windows directory/DNS server and joined a Windows client to the domain. This gave me central account management and a workstation for testing. When domain discovery failed, I corrected the firewall access needed for the join.

3. **Created users and role groups.** I added fictional employee, help desk, and administrator accounts and groups. This let me practice giving people access based on their responsibilities.

4. **Applied and checked Windows access policies.** I organized the client in the intended directory folder, applied Group Policy, and checked sign-in, remote access, and administrator membership. I also removed temporary elevation during later checks to keep the role boundaries clear.

5. **Worked through three help desk scenarios.** I unlocked an account, onboarded a new user, and restored access to a shared resource. These exercises connected the server setup to everyday support work.

6. **Added an Ubuntu server in AWS.** I launched a Linux server with encrypted storage and restricted administration access. This gave me a place to practice Linux and host the help desk portal.

7. **Configured Linux DNS and time.** I made sure Linux could find the Windows directory services and resolve external names, and that its clock was synchronized. I repeated DNS checks after reboot because domain authentication depends on reliable name resolution and time.

8. **Joined Linux to the Windows domain.** I registered the Linux machine in Active Directory and checked domain-user and group lookups. I restricted Linux access to the administration role, tested excluded accounts, and verified the intended administrator could run elevated commands.

9. **Hardened Linux remote access.** I disabled root and password SSH login, checked key-based access, and enabled the Linux firewall. This reduced the ways someone could connect to or administer the server.

10. **Repaired the Linux authentication services.** Some supporting SSSD services failed even while the main service was active. I adjusted the configuration and rechecked services, identity lookups, and domain trust so the join was supported by working authentication services.

11. **Deployed the help desk portal.** I installed the Python/SQLite application under a dedicated service account, used Gunicorn to run it, and put Nginx in front to handle web requests. I configured automatic service startup so the portal could run without an open terminal.

12. **Tested the portal from Windows.** I created and updated a fictional ticket, checked the application and web-proxy health responses, and confirmed the ticket survived service and later instance restarts. This checked the user workflow as well as whether the services were running.

13. **Added local database backups and tested a restore.** I configured validated backups, protection against overlapping runs, and retention of the newest 14 completed copies. I restored a separate database and compared its contents to check that the backup was usable.

14. **Added private backups in S3.** I used an instance role to upload encrypted, versioned cloud copies and configured retention rules. I downloaded a copy, checked its checksum and database contents, and tested anonymous-access denial to check both recovery and privacy.

15. **Repaired the scheduled backup's upload connection.** The first observed scheduled run made a local backup but skipped S3. I restored the missing service hook and verified three complete manual runs. Observing a later unattended run is still pending.

16. **Backed up application files and tested recovered sources.** I archived selected application/configuration files and dependency information, excluding live secrets and the database. Restored file hashes matched, and 12 recovery checks plus four regression tests passed using isolated data and the existing Python environment.

17. **Prepared a separate recovery host.** I selected specific backup versions and checked the proposed image, network, and restricted recovery permissions. The launch dry run passed, but I have not launched or rebuilt that host yet; the purpose is to test recovery beyond the original server.

18. **Organized the portfolio records.** Throughout the build, I recorded results, collected milestone screenshots, and updated GitHub. I redacted identifying details and shortened the documentation so readers can follow the work and distinguish completed checks from plans.

## Where I stopped

The Windows/Linux foundation, portal, and tested backup copies are in place. My next major step is a complete rebuild on a separate host with fresh dependencies, browser/restart checks, and a measured recovery time. I also need to observe an unattended backup after the upload repair and finish the remaining access checks.

Certificates/HTTPS, an intranet page, website domain sign-in and ticket permissions, VPN access, centralized logging/syslog, and automation are planned extensions.

For the next tasks, see the [roadmap](roadmap.md). Supporting details are in [Linux configuration](linux-operations.md), [backup and recovery](backup-and-recovery.md), and the [operations runbook](operations-runbook.md).
