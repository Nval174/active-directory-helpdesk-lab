# Lessons Learned

This is the public learning record for the Windows foundation and Linux extension. Examples are generalized to avoid exposing live infrastructure or identities.

## AWS and Windows Foundation

### Test the layers separately

The Windows client could resolve a directory-server name while domain discovery still failed. Checking DNS, directory services, and connectivity separately isolated the problem to cloud firewall access.

### Role membership and policy both matter

Remote access depends on effective logon rights as well as account/group membership. I corrected a role-membership issue instead of widening access without understanding the cause.

### Computer policy is not user policy

A computer-side policy must target the computer's actual directory location. A standard user's policy results do not necessarily show that computer policy. Later revalidation also showed why live directory state should take precedence over old documentation.

### Authentication is not resource authorization

A fictional support user could sign in but still could not open a shared resource. The cause was a missing NTFS group permission, not the user's password. Restoring minimum read-level access resolved the issue without unnecessary elevation.

### Temporary privilege should be removed

I used elevated access for troubleshooting and removed it afterward. Successful troubleshooting is not a reason to leave extra privileges in place.

## Linux Extension — October 16–17, 2026

### Verify current state

A direct directory query revealed an organizational-unit mismatch in the old notes. I corrected the current-state description instead of treating historical documentation as authoritative.

### Keep an administration path while hardening

I checked SSH syntax and opened another key-based session after applying restrictions. That provided evidence that intended administration still worked before relying on the new configuration.

### Examine failed child units, not only the main service

The main identity process was active while responders showed failures. The host had overlapping responder configuration and socket activation. Backing up and validating the configuration, then checking identities and machine trust again, was more meaningful than clearing the failed-state indicators alone.

### OS identity is not application login

Joining Linux to a directory and delegating sudo by role did not add sign-in or ticket authorization to the web application. These are separate security boundaries.

### Running is not ready

Immediately after restarting the proxy and backend, a request briefly failed even though the processes were active. Follow-up health checks through both paths succeeded. Future automation should use bounded readiness checks and investigate persistent errors.

### Respect the shell boundary

A Windows upload command failed inside the Linux SSH session. Labeling commands by the shell where they run prevents avoidable deployment errors.

### Separate code ownership from data writes

The application runs without root privileges and can write its data location but cannot replace administratively owned application code. Service restrictions add protection; they do not replace application authentication or transport security.

### Backup integrity is only one checkpoint

A consistent SQLite copy passed structural checks. I subsequently restored it into a separate database, compared records, and verified S3 readback and restoration. Those steps provide stronger evidence than backup creation alone. A database-only backup still does not capture a complete rebuild.

### Account for downtime and timezone changes

Persistent timers can catch up after downtime but cannot execute while the host is stopped. A UTC calendar schedule also shifts relative to local wall-clock time when daylight-saving changes.

## Recovery Follow-Up — October 17–18, 2026

### Inspect the loaded job, not just the helper script

A manual uploader worked, but the first observed timer activation only made a local copy. The loaded backup service was missing its cloud upload hook. Inspecting the service's effective configuration exposed that gap. After restoring the hook, complete manual service runs verified both cloud copies and fresh status records. A later unattended run still needs to be observed.

### Recover code as well as data

An application test against restored data can still accidentally rely on the original source tree. I repeated the drill with the actual archived code, templates, and schema held read-only. Its checks passed and the live data stayed unchanged. The drill reused a verified existing dependency environment, so a clean host build remains a separate milestone.

### Keep recovery permissions separate from backup permissions

The application host needs to upload backups. The planned recovery host only needs to read two selected objects and their versions. I used a separate role and IAM simulation to check allowed reads and denied writes, deletes, and unrelated reads. Backup uploads will remain disabled on the isolated recovery host so test data cannot overwrite the live recovery copy.

### Validate the relevant image mapping

The initial image check assumed the AMI had only one block-device mapping. The image also had recognized non-EBS entries. I corrected validation to find the expected root disk by device name while preserving the exact image-identity check and rejecting extra EBS volumes. The revised live preparation passed.

### Distinguish a test result from its surrounding shell session

A pasted closing delimiter was concatenated with another command after a successful application check. I investigated the shell footer separately and rechecked live service health. A passing application test does not make every trailing shell command successful, and a trailing paste error does not by itself invalidate the completed test.

## Documentation and Publication

I keep screenshots at meaningful milestones instead of recording every click. Public summaries should explain the technical reasoning without exposing credentials or live connection details.

I reviewed the screenshot folder alongside the repository history, then replaced identifying text and image versions. I also learned to check what a capture actually proves: the SSSD image showed the original failure, so I kept the successful repair capture pending. The [evidence notes](evidence-notes.md) record these limits.
