# Evidence Notes

I keep screenshots at useful milestones rather than recording every command. I use the build-session results to describe what I completed, and screenshots to support the parts they actually show.

## Linux Milestones

| File | What it records | Status |
|---|---|---|
| [linux-ad-login.png](../pictures/linux-ad-login.png) | An interactive directory-account session and identity checks; identifying output is covered | Added |
| [linux-sudo-access.png](../pictures/linux-sudo-access.png) | The elevated identity check returned `root` | Added |
| [linux-security.png](../pictures/linux-security.png) | SSH settings and host firewall checks, including the final enabled state | Added |
| [sssd-error.png](../pictures/sssd-error.png) | Failed responder units before the repair | Added as failure evidence |
| `pictures/sssd-fix.png` | No failed units, working identity lookups, and machine-trust validation after repair | Capture still needed |
| [helpdesk-service.png](../pictures/helpdesk-service.png) | Database initialization/integrity, enabled and active service, and successful health response | Added |
| [helpdesk-portal.png](../pictures/helpdesk-portal.png) | Browser workflow with a fictional ticket and updated status | Added |

The uploaded SSSD image shows the problem, not the fix. I recorded the successful repair from the follow-up results confirmed during the session. I will not rename the failure image to make it appear to prove success.

I do not have screenshots here proving the completed restart/persistence checks, manually tested backup, or isolated restore. Those results come from the session record.

### Recovery follow-up — October 17–18, 2026

The isolated restore passed SQLite integrity checking. Source and restored copies each contained one ticket, with zero missing rows and zero extra rows in full-row comparisons. The live database was not overwritten.

After all three instances were brought back online, the supplied checks showed the Linux identity, proxy, and application services active; valid directory trust; synchronized time; successful direct/proxied database health; and no failed Linux units. I also confirmed in the Windows browser that the existing ticket remained present.

The subsequent session results established these additional checkpoints:

| Check | Supplied result and scope |
|---|---|
| Local retention | Updated systemd job succeeded with a zero exit status; the installed policy keeps 14 completed copies |
| S3 database round trip | Encryption, versioning, upload checksum, exact downloaded bytes, SQLite integrity, matching ticket count, and anonymous-access denial passed |
| Cloud lifecycle | Daily, latest-recovery, and validation policies inspected; unrelated rules preserved and public access blocking/versioning retained |
| Upload-hook repair | The observed timer activation initially ran only the local backup; the missing hook was restored and three subsequent manual complete-job runs verified both S3 copies |
| Application/configuration archive | 14 application files preserved; all 36 archived files restored and verified after private S3 readback |
| Recovered-source application | Actual recovered source passed 12 recovery checks and four original regression tests; the isolated ticket count changed from one to two while live database contents stayed unchanged |
| Post-drill live health | Application and proxy active; database health successful |
| Replacement-host preparation | Image/network/key-name checks and IAM simulation passed; two object versions pinned; EC2 launch permission dry run passed; no instance launched |

The recovered-source drill used the existing host's Python environment after checking the recorded dependency inventory. It does not establish fresh dependency installation, a complete proxy/backend rebuild, or replacement-host browser access. The archived configuration deliberately excluded the live secret and machine credentials.

A shell paste-footer error appeared after the recovered-source PASS output. I checked the original block and investigated delimiter concatenation, then verified live service health. I kept the application test result separate from the failed trailing shell command.

These results come from the session's terminal output and confirmations. No new recovery screenshots or public raw reports are added here. See the [backup and recovery record](backup-and-recovery.md) for the full sequence and remaining work.

## Screenshot Review

I reviewed 39 current screenshots and one additional image version that appeared only in earlier history. I replaced identifying filenames with simple role-based names and collected the current images in [pictures](../pictures/README.md).

The redactions cover live addresses, network ranges, cloud account/resource IDs, internal host/domain names, account/group names, private paths, and identifying window or browser details. I used solid opaque black rectangles, exported flattened PNGs without the original metadata, and checked that the masked pixels were black. A pixel comparison confirmed that the displayed image outside the masks stayed unchanged. I also reviewed the images visually and used OCR as an additional check.

Some directory screenshots need substantial masking because most of their output consists of account names, groups, and paths. They retain the workflow context but cannot independently prove every statement in the build record. I did not replace hidden text with invented output or turn failures into successful results.

The Windows and networking images are historical evidence. They do not establish the current policy values or complete access-rule set. Where older lockout notes and a screenshot recorded different values, I withheld the exact values instead of guessing which is current.

## Repository History

I rewrote the 77 existing commits to replace the historical screenshot versions and remove identifiers from text, filenames, commit messages, and author/committer metadata. I preserved their order, parent relationships, and dates, then added this documentation and evidence update.

This review covers the files and history included in this repository. It does not establish that external copies have been removed. I keep the original material outside the public repository and do not include raw archives, databases, environment files, or private keys.

## What Remains Unverified

- The database and recovered-source application drills passed; a full replacement-host rebuild and fresh dependency installation remain untested.
- The repaired S3 hook passed manual complete-job runs; a later unattended activation with it loaded has not yet been observed.
- The preparation dry run checks launch permissions; it does not prove instance capacity, successful boot, application readiness, or a measured rebuild time.
- A successful request from the intended client does not prove that every disallowed source is blocked.
- Linux directory membership does not provide web-application authentication or authorization.
- The instance restart follow-up passed the listed readiness and persistence checks; it did not repeat every Windows policy or disallowed-access test.

I will add the missing SSSD capture and any new recovery screenshots when they are available. Certificate services, intranet pages, and web single sign-on remain planned work in the [roadmap](roadmap.md). I do not need extra screenshots for routine package installation, file transfer, or every restart.
