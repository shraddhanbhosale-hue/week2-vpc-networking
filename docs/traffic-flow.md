# AWS VPC Traffic Flow

## Overview

This document explains how network traffic flows through the custom AWS VPC created for Week 2 Task 1.

## VPC Configuration

* VPC CIDR: `10.0.0.0/16`
* Public Subnet: `10.0.1.0/24`
* Private Subnet: `10.0.2.0/24`
* Availability Zone: `eu-north-1a`
* Internet Gateway: Attached to the VPC

## Public Subnet Traffic Flow

The public subnet is associated with the `public-route-table`.

The route table contains:

```text
10.0.0.0/16 → local
0.0.0.0/0   → Internet Gateway
```

When an EC2 instance in the public subnet sends traffic to the internet:

1. The EC2 instance sends the packet through its network interface.
2. The packet reaches the public subnet route table.
3. The route table matches `0.0.0.0/0`.
4. The traffic is forwarded to the Internet Gateway.
5. The Internet Gateway provides connectivity between the VPC and the internet.
6. The response returns through the Internet Gateway to the EC2 instance.

The Week 2 EC2 instance has:

* Private IP: `10.0.1.162`
* Public IP: `16.16.207.245`

Internet connectivity was verified using:

```bash
ping -c 4 8.8.8.8
```

The test returned 4 packets received with 0% packet loss.

## Private Subnet Traffic Flow

The private subnet uses the `private-route-table`.

Its route table contains only:

```text
10.0.0.0/16 → local
```

There is no direct route from the private subnet to the Internet Gateway.

Therefore, resources placed in the private subnet cannot directly communicate with the public internet through the current configuration.

The private subnet can communicate with other resources inside the VPC through the local route.

## Security

The EC2 security group allows:

```text
SSH  → TCP 22
HTTP → TCP 80
```

SSH was used to successfully connect to the public EC2 instance.

## Conclusion

The VPC successfully separates resources into public and private network segments. The public subnet provides internet connectivity through the Internet Gateway, while the private subnet remains isolated from direct internet access.
