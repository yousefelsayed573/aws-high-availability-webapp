# Evidence, Scope, and Publication Notes

## Source of evidence

- Original training assignment: *AWS Hands-on Project - High Availability Web Application* (provided privately to the reviewer; not redistributed).
- Author's completed submission: *AWS_HA_WebApp_YousefElsayed.pdf*, September 2026 (provided privately; omitted because some embedded screenshots expose public addresses, an AWS account identifier, and resource details).
- This repository uses only selected screenshots extracted without visual alteration from the author's submitted report.
- The `scripts/user-data.sh` file was transcribed from the appendix of that report because the standalone source file was unavailable. It is therefore a **report-derived transcription**, not an independent original-file checksum match. The demonstration application was provided in the course instructions, and the author confirmed permission to redistribute it.

## Screenshot evidence

| File | What it supports | Important caveat |
| --- | --- | --- |
| `subnets-across-azs.jpg` | Two public and two private subnets in two AZs | Shows AWS VPC and subnet resource IDs |
| `public-route-table.jpg` | Public route table default route to IGW | Historical VPC, route table, and gateway IDs |
| `private-route-table.jpg` | Private route table default route to NAT | Shows one NAT Gateway shared by private subnet routes |
| `application-security-group.jpg` | App SG permits TCP 8002 from NLB SG and SSH from Bastion SG | Historical SG/resource IDs visible |
| `private-network-acl.jpg` | Custom private NACL and rule ordering | Broad allow-all after SSH rules; not blanket least-privilege enforcement |
| `app1-service-validation.jpg` | App1 cloud-init, systemd, socket, and curl response | Local instance validation, not failover testing |
| `app2-service-validation.jpg` | App2 cloud-init, systemd, socket, and curl response | Local instance validation, not failover testing |
| `nlb-healthy-targets.jpg` | Two healthy registered EC2 targets on port 8002 | Health at capture time only; shows AWS resource IDs |
| `nlb-browser-response.jpg` | Browser received instance-specific app output via NLB | Shows retired NLB public DNS; one response does not prove failover |
| `ssh-to-private-db1.jpg` | SSH session to DB1 via the configured Bastion route | Demonstrates DB1, not independent DB2 login |
| `postgresql-db1-query.jpg` | Active PostgreSQL and a successful demo SQL query on DB1 | DB2 verification is described in the report, but not shown in this image |
| `cloudwatch-cpu-alarm.jpg` | Alarm exists with CPU threshold above 70% | State was `INSUFFICIENT_DATA`; no alarm notification is evidenced |

## What this project does **not** establish

- No demonstrated deliberate failover / chaos test of an unavailable app instance.
- No demonstrated EC2 Auto Scaling Group, automatic instance replacement, or elasticity.
- No PostgreSQL replication, automatic DB failover, or integration between the sample Python HTTP service and either database.
- No infrastructure-as-code (Terraform or CloudFormation) implementation.
- No TLS termination: the demonstrated application endpoint used TCP/80 (HTTP).
- No CloudWatch alarm transition to `ALARM`, notification, or automated action.

The two application EC2 instances were placed in **public subnets**, in accordance with the lab specification. The PostgreSQL EC2 instances were placed in **private subnets**. One NAT Gateway served the private subnet routes from Availability Zone 1, leaving a potential single point of failure for outbound connectivity.

## Security / publication checks

The author confirmed the AWS lab resources were deleted after the assignment and approved publishing historical AWS resource details. Selected screenshots contain historical *internal* IPs, AWS resource IDs, and a retired load balancer DNS name. The persistent AWS account identifier and public EC2 IPv4 addresses are not deliberately included in this selection. Do not include the original course PDF or unredacted submission report in this public repository.

The supplied User Data runs its `systemd` service as `root` because that is what the lab appendix documents. A non-root service account would be a sensible future hardening improvement, but this repository does not claim that such a change was made during the lab.
