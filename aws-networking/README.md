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

<!--<img width="1089" height="516" alt="First VPC" src="https://github.com/user-attachments/assets/ddf7b140-af12-40a0-bb6a-a0b5c508eb9d" />
 -->

### Subnet

<!-- <img width="1060" height="173" alt="Route Table" src="https://github.com/user-attachments/assets/4fa9021c-d8e0-4133-9ac4-288d7feae8bc" />
 -->

### Route Table

<!-- <img width="1109" height="568" alt="Subnet 1" src="https://github.com/user-attachments/assets/f85b4454-7376-4d2d-92e3-4751a60313e0" /> -->

