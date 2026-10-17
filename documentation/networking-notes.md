# Networking Notes

This public edition omits live network ranges, addresses, identifiers, and detailed access rules.

## What I Learned

### Network boundaries and routing

A virtual network defines the lab's network boundary. Subnets divide its address space, and route tables determine forwarding. An internet gateway supplies a connectivity path; it is not, by itself, an access-control policy.

### Layered controls

Security groups and host firewalls are independent layers. A permitted cloud rule does not guarantee that the operating system accepts the connection, and a shared subnet does not mean all traffic is allowed.

### Direction matters

For an inbound rule, I identify the receiving system first and then the initiating source. This helped prevent confusing the directory server's rules with the Linux web server's rules.

### DNS versus complete service reachability

Name resolution, directory discovery, authentication, and application access are different tests. I worked through them separately instead of inferring complete health from one successful lookup.

### Backend isolation

The public-facing process in the logical application design is the reverse proxy. The application backend is bound to loopback and does not need its own network ingress allowance.

## Linux Extension

I made directory-aware DNS persistent, verified service discovery after reboot, and tested the portal from the intended lab client.

The private configuration is kept outside this public summary. See [AWS Networking](../aws-networking/README.md) for the methods and [Architecture](architecture.md) for the role-based conceptual diagram.
