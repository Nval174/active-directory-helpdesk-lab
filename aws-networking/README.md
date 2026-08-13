# AWS Networking

This section documents the AWS network I built for the lab before deploying and configuring the Windows systems.

## What I Built

- VPC: `[lab network resource]` — `[redacted address]`
- Initial public subnet: `[lab network resource]` — `[redacted address]`
- Compute subnet: `[lab network resource]` — `[redacted address]`
- Internet Gateway: `[lab network resource]`
- Route table: `[lab network resource]`
- Default route: `[redacted address]` → Internet Gateway

## Evidence

These screenshots show the network configuration I completed in AWS. I reviewed the images and covered sensitive information before adding them to the repository.

### VPC

I created the VPC as the main network boundary for the lab and used the `[redacted address]` address space.

![VPC configuration](network-vpc.png)

### Initial Public Subnet

I created the initial public subnet using `[redacted address]`.

![Subnet configuration](network-subnet.png)

### Route Table

I associated the public subnet with `[lab network resource]` and configured the local route plus the default route through the Internet Gateway.

![Route table configuration](network-route-table.png)

## Additional Compute Subnet

I later created `[lab network resource]` using `[redacted address]` after the first Availability Zone did not support the EC2 instance configuration I was trying to use. The Windows instances use this subnet.
