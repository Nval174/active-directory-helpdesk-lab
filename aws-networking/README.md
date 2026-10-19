# AWS Networking

I created the lab's virtual network, subnet allocations, internet gateway, and routing. An availability-zone constraint required adjusting the compute deployment location. The Windows systems and Linux extension use the existing compute subnet.

Live addresses, resource IDs, internal names, administrative source addresses, and exact rule sets are omitted.

## Verified Relationships

| Destination | Intended source or path |
|---|---|
| Directory services | Lab systems requiring DNS, discovery, and authentication |
| Linux web proxy | Intended Windows lab client |
| Administrative services | Restricted administration source |
| Application backend | Loopback connection from the proxy |

Cloud security groups and the Linux host firewall enforce separate controls. I checked the compute subnet's explicit route-table association rather than assuming the VPC's main table applied. Its internet-gateway route supplies a path; it does not authorize inbound traffic.

Directory-aware DNS persisted across reboot, and the portal worked from the intended Windows client. Negative web-access tests and review of an older broad directory connectivity allowance remain pending.

## Historical Captures

These images record the original setup, not a current network inventory.

![Virtual-network setup](../pictures/network-vpc.png)

![Subnet setup](../pictures/network-subnet.png)

![Route-table setup](../pictures/network-route-table.png)

See the [architecture](../documentation/architecture.md), [diagnostic notes](../documentation/networking-notes.md), and [evidence notes](../documentation/evidence-notes.md).
