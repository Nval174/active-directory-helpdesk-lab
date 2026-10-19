# Networking Notes

## Diagnostic Method

I check the receiving service, initiating source, and failed layer before changing access rules.

1. Verify addressing and the subnet's effective route table.
2. Test the relevant DNS lookup or directory-service discovery.
3. Check destination cloud rules and the host firewall separately.
4. Confirm that the service is listening on the intended interface.
5. Test the actual authentication or application workflow.

A successful host lookup did not prove domain discovery: the original Windows join problem required correcting cloud firewall access. A shared subnet did not make that traffic automatically permissible.

## Linux and Portal Checks

I made directory-aware DNS persistent, tested internal/external resolution and discovery, and rechecked behavior after reboot.

The client reaches Nginx; Gunicorn listens on loopback. I checked direct backend health, proxied health, and the Windows browser workflow separately. This distinguished a brief startup-readiness failure from a persistent proxy problem.

The [AWS networking record](../aws-networking/README.md) describes the verified network relationships and remaining access tests. Live connection details are kept in private notes.
