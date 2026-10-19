# Evidence Notes

I describe completed work from the recorded session results and use screenshots only for what they visibly show. The [screenshot index](../pictures/README.md) lists the current captures; the [build notes](build-notes.md) summarize the sequence.

## Linux Milestones

The available images show directory-account commands, an elevated identity check, effective SSH settings and firewall state, the original SSSD responder failure, application health, and the portal workflow.

The SSSD repair passed follow-up identity, trust, and failed-unit checks during the session. Its success capture, `pictures/sssd-fix.png`, is still missing; the failure image does not prove the repair.

The security image shows both the initial inactive firewall and its final active state. It also shows effective root/password SSH restrictions. Some identity output is covered, so the images cannot independently establish every reported account check.

## Recovery Evidence — October 17–18, 2026

Database restore comparisons, instance restart checks, S3 verification, archive restoration, recovered-source application tests, and recovery-host preparation are recorded in the [backup and recovery record](backup-and-recovery.md). They come from terminal output and browser confirmations; no new recovery screenshots or raw reports have been published here.

The application drill used recovered source and isolated data, with the existing dependency environment checked against the inventory. The live database stayed unchanged. This does not establish a full new-host rebuild, and manual backup jobs after the upload-hook repair do not prove a later unattended run.

## Screenshot Review

I reviewed 39 current screenshots and one additional image version found only in earlier history. The images use role-based filenames.

Redactions cover live addresses/ranges, cloud account/resource IDs, internal names, accounts/groups, private paths, and identifying window/browser content. I used opaque masks, exported flattened PNGs without the original metadata, verified masked pixels, and compared the unmasked image regions. Visual review and OCR supplied additional checks.

I did not invent replacement output. Historical Windows/network captures do not prove current policy values, membership, or a complete access-rule inventory. Where lockout notes and a capture differed, I withheld the exact values.

## Repository History and Source

I replaced identifying screenshots and text across 77 earlier commits while preserving their sequence, parent relationships, and dates. The later recovery documentation commit uses GitHub's no-reply address for author and committer. External cached copies and clones may retain older material.

The repository currently contains documentation and redacted images. The deployed application, tests, service/proxy templates, and backup helpers are not yet versioned here. Private recovery packages are separate; they exclude the live secret and machine credentials.

## Remaining Evidence

- A successful SSSD repair capture and useful recovery-drill captures
- A fresh-host rebuild with fresh dependency installation, full web deployment, browser access, and measured recovery time
- An unattended complete backup after the cloud-hook repair
- Negative web-access tests and review of the older broad connectivity allowance
- Reviewed, sanitized application/configuration source and tests

The latest restart checks did not repeat every Windows policy or denied-access test. Application authentication, HTTPS, certificate services, and domain web sign-in remain planned work in the [roadmap](roadmap.md).
