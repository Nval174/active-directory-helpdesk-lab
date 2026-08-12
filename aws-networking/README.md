# AWS Networking

This section covers the AWS network I built for the lab before deploying the Windows machines.

## What I Built

- VPC: `AD-Helpdesk-Lab-VPC` — `10.0.0.0/16`
- Public subnet: `AD-Helpdesk-Public-Subnet` — `10.0.0.0/20`
- Internet Gateway: `AD-Helpdesk-IGW`
- Route table: `AD-Helpdesk-Public-RT`
- Default route: `0.0.0.0/0` → Internet Gateway

## Evidence

### VPC

<!-- IMAGE PLACEHOLDER: add the redacted VPC screenshot here as `vpc.png`. -->

### Subnet

<!-- IMAGE PLACEHOLDER: add the redacted subnet screenshot here as `subnet.png`. -->

### Route Table

<!-- IMAGE PLACEHOLDER: add the redacted route table screenshot here as `route-table.png`. -->

The screenshots are meant to support the written explanation, so each image will sit next to the configuration it proves rather than being placed in a separate screenshot dump.
