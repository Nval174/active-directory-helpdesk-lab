# Networking Notes

These are the networking ideas that stood out to me while building the AWS side of the lab.

## VPC vs. Subnet

The VPC is the main network boundary for the lab and uses `10.0.0.0/16`.

The first subnet I created uses `10.0.0.0/20`. I later created `10.0.16.0/20` as a separate compute subnet for the Windows instances.

## CIDR

The `/16` and `/20` prefixes determine the size of the IPv4 networks. Understanding those ranges helped me verify that DC01's `10.0.22.196` address belongs to the `10.0.16.0/20` compute subnet.

## Internet Gateway

The Internet Gateway gives the VPC a path to the internet. It does not, by itself, mean every resource is reachable from the internet. Routing and security controls still matter.

## Route Table

The public route table currently has:

- `10.0.0.0/16` → `local`
- `0.0.0.0/0` → `AD-Helpdesk-IGW`

The local route handles traffic inside the VPC. The `0.0.0.0/0` route is the default IPv4 route for destinations outside the VPC.

## Public Subnet and Compute Subnet

I enabled automatic public IPv4 assignment on the subnets used by the lab so I can remotely administer the Windows systems. The EC2 security groups are what limit which inbound traffic is actually allowed.

## Security and Cost

I'm treating internet access as something that needs to be controlled rather than assumed to be safe. I keep administrative access restricted to the traffic the lab actually needs, and I keep the AWS resources as small and short-lived as practical so I don't burn through credits unnecessarily.
