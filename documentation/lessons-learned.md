# Lessons Learned

## Diagnose the layer that failed

The Windows client could resolve the directory server while domain discovery still failed. Checking DNS, discovery, and connectivity separately exposed the cloud firewall problem. Later, I used the same approach to distinguish backend, proxy, and browser failures.

Remote access required checking effective logon rights and group membership. Shared-resource access required NTFS permissions even when sign-in worked. I corrected the specific permission instead of granting broader privileges.

Computer policy also depends on the computer object's actual directory placement. A later live query corrected an organizational-unit mismatch in my old notes.

## Verify administration after hardening

I checked SSH syntax and opened another key-based session after applying restrictions. I also removed temporary elevated Windows access after troubleshooting.

The portal runs under a dedicated account that can write its data location but cannot replace application code. Joining Linux to the domain and delegating sudo did not add web login or ticket authorization.

## Active does not always mean healthy

The main SSSD service was active while responder units failed. I corrected overlapping responder/socket activation, then checked identities, trust, and failed units; clearing failure indicators alone would not establish a repair.

A web request briefly failed immediately after the proxy/backend restart, while subsequent direct and proxied health checks passed. Future automation needs bounded readiness checks rather than process status alone.

I also learned to label commands by shell: a Windows file-transfer command failed when entered inside the SSH Bash session.

## Test restoration, not only backup creation

Integrity checking established a structurally sound database copy. Separate record comparisons, private S3 retrieval, and recovered-source application tests established more useful recovery evidence.

The first observed timer activation still missed cloud upload because the loaded service lacked its hook. Testing the uploader alone was insufficient; I had to inspect the effective unit and rerun the complete job.

Persistent timers cannot run on stopped hosts, and UTC schedules shift relative to local time across daylight-saving changes.

## Keep recovery independent

I repeated the application drill with archived code, templates, and schema held read-only, rather than accidentally importing the original source tree. It passed with isolated data and a verified existing dependency environment. A fresh-host rebuild remains a separate milestone.

The backup host needs write access, while the recovery role only needs selected reads. IAM simulation checked that distinction. Uploads must stay disabled on the recovery host so test records cannot replace the live recovery copy.

The image validator initially assumed one block-device mapping. I corrected it to identify the expected root EBS disk while accepting recognized non-EBS entries and still rejecting unexpected EBS disks.

## Match the claim to the evidence

The SSSD screenshot records the original failure; it does not show the successful repair. I kept that capture pending instead of relabeling the failure.

The same distinction applies to a launch dry run versus a booted host, and to private restored sources versus source published in Git. I keep verified outcomes, missing evidence, and planned work explicit in the [evidence notes](evidence-notes.md) and [roadmap](roadmap.md).
