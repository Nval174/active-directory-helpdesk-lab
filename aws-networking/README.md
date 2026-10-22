# AWS Networking

I created the VPC, subnet allocations, internet gateway, and routing. The directory server, Windows client, and Ubuntu server use the existing compute network.

## Access paths

| Destination | Configured or intended path |
|---|---|
| Directory/DNS services | Lab systems requiring discovery and authentication |
| Nginx | Intended Windows lab client |
| Administration | Restricted administration source |
| Gunicorn | Loopback connection from Nginx |

I checked the subnet's effective route-table association. Cloud security groups and the Linux firewall were checked separately. Directory-aware DNS survived reboot, and the intended client reached the portal.

Tests from disallowed web sources and review of an older broad directory-connectivity rule remain pending. The paths above are not a completed denial-test matrix.

## Troubleshooting order

1. Check addressing, the effective route, and the relevant DNS/service-discovery query.
2. Check the destination's cloud rules and host firewall.
3. Confirm the listener and test the actual authentication/application workflow.

The original domain-join failure required correcting firewall access even though the directory-server name resolved. For the portal, I checked backend health, proxied health, and browser access separately.

## Historical captures

[Virtual network](../pictures/network-vpc.png) · [Subnet](../pictures/network-subnet.png) · [Route table](../pictures/network-route-table.png)

These record the original setup. Live addresses and rule sources are omitted. See [architecture](../documentation/architecture.md) and [evidence notes](../documentation/evidence-notes.md).
