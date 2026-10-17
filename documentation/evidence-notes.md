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

I do not have screenshots here proving the completed restart/persistence checks or the manually tested backup. Those results come from the session record. I have not completed a restore exercise.

## Screenshot Review

I reviewed 39 current screenshots and one additional image version that appeared only in earlier history. I replaced identifying filenames with simple role-based names and collected the current images in [pictures](../pictures/README.md).

The redactions cover live addresses, network ranges, cloud account/resource IDs, internal host/domain names, account/group names, private paths, and identifying window or browser details. I used solid opaque black rectangles, exported flattened PNGs without the original metadata, and checked that the masked pixels were black. A pixel comparison confirmed that the displayed image outside the masks stayed unchanged. I also reviewed the images visually and used OCR as an additional check.

Some directory screenshots need substantial masking because most of their output consists of account names, groups, and paths. They retain the workflow context but cannot independently prove every statement in the build record. I did not replace hidden text with invented output or turn failures into successful results.

The Windows and networking images are historical evidence. They do not establish the current policy values or complete access-rule set. Where older lockout notes and a screenshot recorded different values, I withheld the exact values instead of guessing which is current.

## Repository History

I rewrote the 77 existing commits to replace the historical screenshot versions and remove identifiers from text, filenames, commit messages, and author/committer metadata. I preserved their order, parent relationships, and dates, then added this documentation and evidence update.

This review covers the files and history included in this repository. It does not establish that external copies have been removed. I keep the original material outside the public repository and do not include raw archives, databases, environment files, or private keys.

## What Remains Unverified

- A database integrity check does not prove a successful restore or recovered records.
- An enabled timer and successful manual backup do not prove a future unattended run.
- A successful request from the intended client does not prove that every disallowed source is blocked.
- Linux directory membership does not provide web-application authentication or authorization.
- Service restarts do not replace a complete instance stop/start recovery test.

I will add the missing SSSD capture and a restore milestone only after carrying out those checks. I do not need extra screenshots for routine package installation, file transfer, or every restart.
