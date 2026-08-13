# AWS Networking

This section documents the AWS network I built for the lab before deploying and configuring the Windows systems.

## What I Built

- VPC: `AD-Helpdesk-Lab-VPC` — `10.0.0.0/16`
- Initial public subnet: `AD-Helpdesk-Public-Subnet` — `10.0.0.0/20`
- Compute subnet: `AD-Helpdesk-Compute-Subnet` — `10.0.16.0/20`
- Internet Gateway: `AD-Helpdesk-IGW`
- Route table: `AD-Helpdesk-Public-RT`
- Default route: `0.0.0.0/0` → Internet Gateway

## Evidence

These screenshots show the network configuration I completed in AWS. I reviewed the images and covered sensitive information before adding them to the repository.

### VPC

I created the VPC as the main network boundary for the lab and used the `10.0.0.0/16` address space.

![VPC configuration](VPC.png)

### Initial Public Subnet

I created the initial public subnet using `10.0.0.0/20`.

![Subnet configuration](Subnet.png)

### Route Table

I associated the public subnet with `AD-Helpdesk-Public-RT` and configured the local route plus the default route through the Internet Gateway.

![Route table configuration](route-table.png)

## Additional Compute Subnet

I later created `AD-Helpdesk-Compute-Subnet` using `10.0.16.0/20` after the first Availability Zone did not support the EC2 instance configuration I was trying to use. The Windows instances use this subnet.
