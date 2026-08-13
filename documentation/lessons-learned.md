# Lessons Learned

This is where I'm keeping the technical lessons that have stood out to me while building and troubleshooting the lab.

## AWS Networking

### VPC vs. Subnet

I learned that the VPC is the main network boundary for the lab, while subnets divide that address space into smaller networks. My VPC uses `10.0.0.0/16` and my initial public subnet uses `10.0.0.0/20`.

I later created a second subnet, `10.0.16.0/20`, for the Windows compute instances after the first Availability Zone did not support the instance configuration I was trying to use.

### CIDR Notation

The `/16` and `/20` prefixes determine the size of the IPv4 networks. Understanding the ranges helped me recognize that DC01's address, `10.0.22.196`, was valid inside the `10.0.16.0/20` compute subnet.

### Internet Gateway and Route Tables

I learned that an Internet Gateway provides a path between the VPC and the internet, while the route table determines where traffic is sent. The default route `0.0.0.0/0` points to the Internet Gateway, while the local route handles traffic inside the VPC.

### Security Groups

I learned that connectivity problems can come from more than one layer. In this lab, DNS could work while other Active Directory traffic was blocked. Testing the required ports helped me identify that the DC01 security group needed additional access from the CLIENT01 security group.

I deliberately kept the source limited to the CLIENT01 security group instead of opening the Active Directory traffic to the public internet. I also learned that a broad rule can be useful during a lab build, but it should be reviewed and tightened during hardening.

## Active Directory and DNS

### DNS Matters to Active Directory

One of the biggest lessons so far has been how much Active Directory depends on DNS. CLIENT01 could resolve the domain controller, but that alone was not enough for domain-controller discovery.

### Troubleshooting `nltest`

Before joining CLIENT01 to the domain, `nslookup` could resolve DC01 but `nltest /dsgetdc:corp.local` returned `ERROR_NO_SUCH_DOMAIN` (1355).

I checked the DNS service, confirmed the `corp.local` zone existed, verified the Active Directory DNS records, checked Netlogon, ran the DNS diagnostic on DC01, and tested the required connectivity from CLIENT01. The issue was ultimately tied to missing client-to-DC connectivity in the AWS security group. After correcting the rules, the connectivity tests succeeded and `nltest` was able to find DC01.

That was a useful reminder that troubleshooting should move through the layers instead of assuming that a successful DNS lookup means the entire Active Directory connection is healthy.

## Group Policy

I learned that the computer and the user are separate policy targets. `Lab-Workstations-Baseline` is a computer-side GPO linked to the `Lab-Workstations` OU, so it applies to CLIENT01 rather than directly to John Smith.

I also learned that the computer object has to be in the OU where the GPO is linked. CLIENT01 initially wasn't in that OU, so the policy did not appear in the computer results until I moved it and refreshed Group Policy.

## Help Desk / Troubleshooting

The first account-lockout scenario helped me practice a simple support workflow: confirm the user's account state, identify the likely cause, make the smallest administrative change needed, and verify that the user can log in again.

## Cost Management

I'm keeping the AWS lab small because I want the project to stay useful without spending credits unnecessarily. I'm using smaller instances where practical and plan to stop resources when I'm done working with them.

## Documentation

I'm keeping screenshots at meaningful verification points rather than capturing every click. I also review screenshots before adding them to the repository so I don't accidentally publish credentials or other sensitive information.
