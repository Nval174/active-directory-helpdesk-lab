# Lessons Learned

This document captures technical concepts, mistakes, troubleshooting discoveries, and practical lessons learned throughout the lab.

## AWS Networking

### VPC vs. Subnet

A VPC provides the overall network boundary and address space for the AWS lab. The lab VPC uses `10.0.0.0/16`.

A subnet is a smaller network segment inside the VPC. The lab uses `10.0.0.0/20` for the initial public subnet.

### CIDR Notation

The `/16` and `/20` prefixes determine the size of the corresponding IPv4 network. The `/20` subnet is contained within the `/16` VPC address space.

### Internet Gateway

An Internet Gateway provides a path between a VPC and the internet. Attaching an Internet Gateway to the VPC does not by itself make every resource publicly accessible; routing and security controls still determine how traffic can flow.

### Route Tables

A route table determines where traffic from an associated subnet is directed. The lab's public route table contains a local route for the VPC and a default IPv4 route (`0.0.0.0/0`) pointing to the Internet Gateway.

### Public Subnet

For this lab, the subnet is configured to automatically assign public IPv4 addresses. This will support remote administration of the lab systems, while security groups will later be used to restrict inbound traffic.

## Security Considerations

A network having internet connectivity does not mean that all traffic should be allowed. Before Windows Server instances are deployed, the lab will use restrictive security group rules and only expose the traffic required for administration and lab functionality.

## Cost Management

AWS credits are available for the lab, but minimizing unnecessary spending is a project requirement. Compute and other potentially billable resources will be reviewed before deployment, and resources will not be left running unnecessarily.

## Documentation Practice

Screenshots are being collected at meaningful verification points rather than documenting every click. Screenshots will be reviewed for sensitive information before being added to the public GitHub repository.
