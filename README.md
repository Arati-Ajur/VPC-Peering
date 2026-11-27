# AWS VPC Peering Project

This project connects two VPCs in **us-east-1 (N. Virginia)** using a **VPC peering connection**, so EC2 instances in each VPC can talk to each other over private IP addresses. The connection is tested with `ping`.

Built as part of the **EWB Courses AWS Architect Associate** training (Project 01).

## Architecture

| Component | VPC-A | VPC-B |
|---|---|---|
| VPC name | EWB-Vpc-a | EWB-VPC-b |
| CIDR block | 10.10.0.0/16 | 10.20.0.0/16 |
| Subnet | EWB-Public-a | EWB-public-b |
| Availability Zone | us-east-1a | us-east-1b |
| Internet gateway | EWB-IGW-a | EWB-IGW-b |
| Route table | EWB-RT-a | EWB-RT-b |
| EC2 instance | EWB-Instance-a (10.10.0.173) | EWB-Instance-b (10.20.0.185) |

Peering connection: **EWB-VPC-peering** (`pcx-04a8b5d66638d7634`)

> The two CIDR blocks must not overlap, or peering will fail.

## Steps performed

1. Created VPC-A (10.10.0.0/16) and a subnet in it.
2. Created and attached an internet gateway, then a route table with a `0.0.0.0/0` route.
3. Created VPC-B (10.20.0.0/16) with its own subnet, internet gateway and route table.
4. Launched one `t3.micro` EC2 instance in each VPC.
5. Created a peering connection from VPC-A to VPC-B and accepted it (status: Active).
6. Added a route in each route table to the other VPC's CIDR through the peering connection.
7. Connected to EWB-Instance-a with EC2 Instance Connect and ran `ping 10.20.0.185`.

## Result

Ping from Instance-a to Instance-b over the private IP succeeded with about 0.5 ms latency, which confirms the VPCs communicate privately through the peering connection.

## What I learned

- A VPC is an isolated network with its own CIDR range.
- VPC peering connects two VPCs privately, without using the internet.
- Route tables must be updated on **both** sides.
- Security groups must allow the traffic (SSH and ICMP).
- Peering is non-transitive.


## Author

Arati Ajur
