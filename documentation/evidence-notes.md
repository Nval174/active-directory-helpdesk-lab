# Evidence Notes

Evidence should show a meaningful configuration, investigation, or verification result. I do not need a screenshot for every command or routine task.

## Linux Milestone Placeholders

The following filenames are reserved for the October 16–17, 2026 build. **Uploads are pending.** These are text placeholders, not images that have already been added.

| Reserved path | Useful evidence | Status |
|---|---|---|
| `pictures/linux-ad-login.png` | Successful Linux domain-account session with identity details redacted | Upload pending |
| `pictures/linux-sudo-access.png` | Role-based elevated-command result without account/group identifiers | Upload pending |
| `pictures/linux-security.png` | SSH/firewall verification with live addresses and exact source details covered | Upload pending |
| `pictures/sssd-fix.png` | Failed units cleared and identity/trust checks working after repair | Upload pending |
| `pictures/helpdesk-service.png` | Active application service, isolated backend, and successful health result | Upload pending |
| `pictures/helpdesk-portal.png` | Working portal using fictional sample content and a redacted browser address | Capture confirmed; upload pending |

Upload sanitized copies using these exact filenames. Then replace the matching text placeholders in the build write-up with relative image references.

No additional screenshot is required for file transfer, package installation output, routine restart checks, or every backup command. Future restore evidence should be added only after the exercise is actually completed.

## Redaction Checklist

Before uploading any image, inspect the whole frame, including title bars, terminal prompts, browser addresses, navigation panels, and account menus.

Remove or cover:

- Passwords, keys, tokens, session cookies, recovery material, and configuration secrets
- Public/private addresses, network ranges, internal host/domain names, and service URLs
- Cloud account/resource identifiers, key-pair names, and detailed access-rule sources
- Lab account/group identifiers, private directory paths, and operational identifiers
- Personal information and real customer/employer data

Retain enough context to demonstrate the result. Do not modify a failure to make it look like success or fabricate evidence.

## Existing Images and Git History

Older screenshots remain in the repository, including the original networking images. **They have not been re-audited as part of this text-only update.** Old captions claiming prior review should not be treated as a current privacy certification.

This commit sanitizes the current Markdown, not earlier Git history, cached copies, or image pixels. Removing references from a page does not remove the underlying image.

If a credential is ever found in any version or image, revoke/rotate it promptly; deleting the visible text is not sufficient. A separate review should determine whether image replacement and repository-history cleanup are needed.

## Verification Scope

The build record uses command output and confirmed results from the session. Pending screenshots have not been visually reviewed.

A database integrity check is not a restore test. A successful request from one permitted client is not a negative access test from other sources. A configured timer is not proof that a future unattended run has completed.
