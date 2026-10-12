# Cloud Operations Lab — Architecture

## Overview

The lab uses a custom Amazon VPC to host an Ubuntu EC2 instance. The instance is used for monitoring, backup and restoration exercises, and service incident recovery.

## Components

| Component | Purpose |
|---|---|
| Amazon VPC | Provides an isolated virtual network |
| Public subnet | Hosts the EC2 instance |
| Internet gateway | Provides an internet route for the public subnet |
| Route table | Routes internet-bound traffic through the internet gateway |
| EC2 security group | Restricts SSH access to the configured client IP |
| Ubuntu EC2 instance | Runs the lab workloads and operational tests |
| IAM instance role | Grants the instance permissions for supported AWS services |
| Systems Manager | Provides managed-instance connectivity |
| CloudWatch | Collects monitoring metrics and evaluates the CPU alarm |
| CloudWatch Agent | Publishes memory and disk usage metrics |
| Nginx | Provides the service used for the incident simulation |

## Network Configuration

- VPC CIDR: `10.0.0.0/16`
- Public subnet CIDR: `10.0.13.0/24`
- Internet route: `0.0.0.0/0` through the internet gateway
- SSH: TCP port 22, restricted to the configured client IP address
- Local Nginx testing: performed through `localhost` on the EC2 instance

## Monitoring Configuration

The CloudWatch Agent publishes the following custom metrics under the `CloudOps-Lab` namespace:

- `mem_used_percent`
- `disk_used_percent`

The `CloudOps-Lab-High-CPU` alarm monitors EC2 CPU utilization with a threshold above 70% over a five-minute period.

## Operational Tests

1. Verified that memory and disk metrics appeared in CloudWatch.
2. Created a compressed archive of a sample file.
3. Restored the archive into a separate directory and verified file integrity.
4. Stopped Nginx to simulate a service outage.
5. Confirmed that the local HTTP request failed while Nginx was stopped.
6. Restarted Nginx and verified an HTTP `200 OK` response.

## Security Notes

SSH access was restricted to a specific client IP address. The lab did not require public HTTP access for the Nginx recovery exercise. No credentials or private keys should be stored in this repository.
