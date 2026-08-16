# Lessons Learned

This is where I'm keeping the technical lessons that have stood out to me while building and troubleshooting the lab.

## AWS Networking

### VPC vs. Subnet

I learned that the VPC is the main network boundary for the lab, while subnets divide that address space into smaller networks. My VPC uses `[redacted address]` and my initial public subnet uses `[redacted address]`.

I later created a second subnet, `[redacted address]`, for the Windows compute instances after the first Availability Zone did not support the instance configuration I was trying to use.

### CIDR Notation

The `/16` and `/20` prefixes determine the size of the IPv4 networks. Understanding the ranges helped me recognize that directory server's address, `[redacted address]`, was valid inside the `[redacted address]` compute subnet.

### Internet Gateway and Route Tables

I learned that an Internet Gateway provides a path between the VPC and the internet, while the route table determines where traffic is sent. The default route `[redacted address]` points to the Internet Gateway, while the local route handles traffic inside the VPC.

### Security Groups

I learned that connectivity problems can come from more than one layer. In this lab, DNS could work while other Active Directory traffic was blocked. Testing the required ports helped me identify that the directory server security group needed additional access from the Windows client security group.

I deliberately kept the source limited to the Windows client security group instead of opening the Active Directory traffic to the public internet. I also learned that a broad rule can be useful during a lab build, but it should be reviewed and tightened during hardening.

## Active Directory and DNS

### DNS Matters to Active Directory

One of the biggest lessons so far has been how much Active Directory depends on DNS. Windows client could resolve the domain controller, but that alone was not enough for domain-controller discovery.

### Troubleshooting `nltest`

Before joining Windows client to the domain, `nslookup` could resolve directory server but `nltest /dsgetdc:[lab domain]` returned `ERROR_NO_SUCH_DOMAIN` (1355).

I checked the DNS service, confirmed the `[lab domain]` zone existed, verified the Active Directory DNS records, checked Netlogon, ran the DNS diagnostic on directory server, and tested the required connectivity from Windows client. The issue was ultimately tied to missing client-to-DC connectivity in the AWS security group. After correcting the rules, the connectivity tests succeeded and `nltest` was able to find directory server.

That was a useful reminder that troubleshooting should move through the layers instead of assuming that a successful DNS lookup means the entire Active Directory connection is healthy.

## Group Policy

I learned that the computer and the user are separate policy targets. `[workstation policy or OU]` is a computer-side GPO linked to the `[workstation policy or OU]` OU, so it applies to Windows client rather than directly to the lab employee.

I also learned that the computer object has to be in the OU where the GPO is linked. Windows client initially wasn't in that OU, so the policy did not appear in the computer results until I moved it and refreshed Group Policy.

The GPO also became the cause of an RDP logon problem during the lab. The detailed Group Policy report showed that `[workstation policy or OU]` controlled the **Allow log on through Remote Desktop Services** setting and only allowed the `[support role]` and `[IT role]` groups. The Administrator account was not in the [IT role] group at first, so RDP access for that account failed. Adding the account to the appropriate role group restored access without weakening the GPO.

## Help Desk / Troubleshooting

The first account-lockout scenario helped me practice a simple support workflow: confirm the user's account state, identify the likely cause, make the smallest administrative change needed, and verify that the user can log in again.

The resource-access ticket reinforced the difference between authentication and authorization. the support employee could log in successfully and was a member of `[support role]`, but she still received Access Denied when the group's NTFS permission was removed from `[shared resource]`. Restoring the group's read-level NTFS permission fixed the problem.

I also temporarily granted the support employee local administrator access during troubleshooting and then removed it. This reinforced that elevated privileges should be temporary when possible and should not be left in place when the task is complete.

## Cost Management

I'm keeping the AWS lab small because I want the project to stay useful without spending credits unnecessarily. I'm using smaller instances where practical and plan to stop resources when I'm done working with them.

## Documentation

I'm keeping screenshots at meaningful verification points rather than capturing every click. I also review screenshots before adding them to the repository so I don't accidentally publish credentials or other sensitive information.