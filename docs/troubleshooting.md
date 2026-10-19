# Bastion Host SSH Timeout: Troubleshooting Record

This incident is documented in the author's completed AWS training submission. It occurred during the lab; these are not instructions to change a currently running environment.

## Symptom

An SSH connection to the public Bastion Host timed out even though the instance was running.

## Investigation

The author checked multiple layers before identifying the cause:

1. EC2 instance state and reachability.
2. VPC routing and public subnet association.
3. Bastion Security Group SSH inbound rule.
4. Public network ACL ingress and egress behavior.
5. Guest operating-system firewall configuration (`ufw`, `nftables`).
6. SSH service/socket configuration (`ssh.socket`).
7. A temporary Systems Manager session to inspect the Bastion directly.
8. A `tcpdump` capture to check whether SSH packets were arriving at the instance.

The packet capture showed **no incoming SSH traffic** while the connection attempt timed out, indicating a block before the Bastion operating system.

## Root cause and resolution

The administrator's public IPv4 address changed following a router/ISP session change. The Bastion Security Group and the relevant network ACL rule still permitted the previous administrative `/32` source address.

After updating the rules to the current administrative `/32`, SSH connectivity worked again.

## Lesson learned

When SSH times out, check the network path and packet arrival before changing server-side SSH settings. Restricting management access to a `/32` is sensible for a lab, but the authorized source address may change on consumer internet connections.
