# Linux Configuration and Troubleshooting

Build: **October 16–17, 2026**.

## Directory integration

I deployed Ubuntu 24.04 LTS with encrypted storage and hardened instance metadata settings. I configured persistent naming and directory-aware DNS, checked internal/external resolution and time synchronization, and repeated DNS checks after reboot.

I created a Linux organizational unit and joined AD using realmd/adcli and SSSD. Membership listing, identity/group lookup, computer registration, and machine-trust checks passed.

Linux access was restricted to the administration role. The intended administrator was allowed; excluded employee/support identities were denied. I enabled home-directory creation, checked sudoers syntax, and verified that the elevated identity check returned `root`. These checks used interactive domain-account sessions, not direct domain-account SSH.

![Elevated identity check](../pictures/linux-sudo-access.png)

The [directory-session capture](../pictures/linux-ad-login.png) retains command context; its identifying output is covered.

## SSH and firewall

I disabled root/password SSH login, reduced authentication attempts, and disabled unused forwarding. Syntax validation and a second key-based connection passed. I enabled the host firewall with the intended lab allowances.

![SSH settings and firewall checks](../pictures/linux-security.png)

## SSSD responder failure

The main SSSD service was active while responder services/sockets failed. I backed up the configuration, adjusted the responder/socket settings, restarted SSSD, and rechecked failed units, identity lookup, and machine trust. Those checks passed.

![Responder failures before repair](../pictures/sssd-error.png)

The successful repair screenshot, `sssd-fix.png`, is still missing.

## Portal deployment

I deployed the Flask/SQLite starter through a Python virtual environment, Gunicorn, systemd, and Nginx. It runs as a dedicated non-login account with a writable data directory and a protected environment file.

I verified the transferred bundle's checksum, initialized SQLite as the application account, checked database integrity and the backend listener, and validated Nginx configuration. From Windows, I created and updated a fictional ticket. It remained after service and later instance restarts.

![Database checks, service state, and health response](../pictures/helpdesk-service.png)

A Windows upload command initially failed inside the Linux SSH session. Rerunning it from local PowerShell resolved the shell mismatch.

A health request immediately after restarting Nginx and Gunicorn returned 502. Later direct and proxied requests succeeded without a configuration change. Startup timing is a possible explanation; I did not establish a definitive cause.

See [backup and recovery](backup-and-recovery.md) for test results and [evidence notes](evidence-notes.md) for what is available to review.
