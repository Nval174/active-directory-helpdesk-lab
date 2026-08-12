# Networking Notes

This file captures a few of the networking ideas that stood out to me while building the AWS side of the lab.

## VPC vs. Subnet

The VPC is the main network boundary for the lab and uses `10.0.0.0/16`.

The subnet is a smaller section inside that VPC. I used `10.0.0.0/20` for the initial public subnet.

## CIDR

The `/16` and `/20` prefixes control how large each IPv4 network is. The `/20` subnet fits inside the `/16` VPC address space.

## Internet Gateway

The Internet Gateway gives the VPC a path to the internet. It does not, by itself, mean every resource is reachable from the internet. Routing and security controls still matter.

## Route Table

The public route table currently has:

- `10.0.0.0/16` → `local`
- `0.0.0.0/0` → `AD-Helpdesk-IGW`

The local route handles traffic inside the VPC. The `0.0.0.0/0` route is the default IPv4 route for destinations outside the VPC.

## Public Subnet

The subnet is configured to automatically assign public IPv4 addresses. This will make remote administration possible later, but the EC2 security groups will still need to restrict which inbound traffic is allowed.

## Security and Cost

I'm treating internet access as something that needs to be controlled rather than assumed to be safe. Before launching the Windows servers, I'll set up security group rules deliberately and keep the AWS resources as small and short-lived as practical.
