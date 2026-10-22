# Evidence Notes

I distinguish recorded session results from what the published files visibly establish.

## Available evidence

- The [screenshot index](../pictures/README.md) links 35 captures from the Windows/Linux build.
- The [recovery record](backup-and-recovery.md) summarizes supplied terminal results and browser confirmations. Raw recovery output, application tests, configuration, and archives are not published here, so those results cannot yet be independently reproduced from this repository.
- The SSSD failure is captured. Follow-up repair checks passed during the session, but `sssd-fix.png` is still missing.
- Identity redactions hide some command output. The security capture includes both the initial inactive firewall and the final active state.

I omitted four captures whose redactions concealed the result: domain login, workstation policy linking, the shared folder, and restored share access. The successful access check remains a session result, not visible screenshot proof.

## Historical limits

Windows/network captures record earlier steps. They do not establish current memberships, policy application, or a complete rule inventory. Older lockout notes and a capture disagree on exact values; I have not inferred which is current.

The recovery dry run and existing-environment tests do not establish a replacement-host rebuild.

## Publication

I keep live connection details, account identifiers, credentials, and private recovery material outside the public files. The screenshots use opaque redactions; their visible content was preserved outside the masks.

The [roadmap](roadmap.md) tracks missing captures, source publication, and unfinished acceptance checks. Future PKI, intranet, VPN, and logging work is planned.
