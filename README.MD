✅ Solution Options (Best → Worst)
🥇 Option 1: VPC Peering (Most common)

Create a VPC Peering connection between:

Default VPC
roboshop-dev VPC

Steps:
Create VPC peering in AWS:

Requester: default VPC
Accepter: roboshop-dev VPC

Update route tables:

Default VPC route table:
destination: roboshop-dev CIDR
target: pcx-xxxx

roboshop-dev route table:
destination: default VPC CIDR
target: pcx-xxxx

Update security groups:
Allow Jenkins agent SG → EKS control plane / node SG