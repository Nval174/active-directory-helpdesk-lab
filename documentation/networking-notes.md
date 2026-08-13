# Networking Notes

These are the networking ideas that stood out to me while building the AWS side of the lab.

## VPC vs. Subnet

The VPC is the main network boundary for the lab and uses `[redacted address]`.

The first subnet I created uses `[redacted address]`. I later created `[redacted address]` as a separate compute subnet for the Windows instances.

## CIDR

The `/16` and `/20` prefixes determine the size of the IPv4 networks. Understanding those ranges helped me verify that directory server's `[redacted address]` address belongs to the `[redacted address]` compute subnet.

## Internet Gateway

The Internet Gateway gives the VPC a path to the internet. It does not, by itself, mean every resource is reachable from the internet. Routing and security controls still matter.

## Route Table

The public route table currently has:

- `[redacted address]` → `local`
- `[redacted address]` → `[lab network resource]`

The local route handles traffic inside the VPC. The `[redacted address]` route is the default IPv4 route for destinations outside the VPC.

## Public Subnet and Compute Subnet

I enabled automatic public IPv4 assignment on the subnets used by the lab so I can remotely administer the Windows systems. The EC2 security groups are what limit which inbound traffic is actually allowed.

## Security and Cost

I'm treating internet access as something that needs to be controlled rather than assumed to be safe. I keep administrative access restricted to the traffic the lab actually needs, and I keep the AWS resources as small and short-lived as practical so I don't burn through credits unnecessarily.
