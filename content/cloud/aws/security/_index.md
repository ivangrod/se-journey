+++
title = "AWS - Security"
date = 2020-04-19T10:49:11+02:00
weight = 2
+++

**IAM** - Identity and Access Management. Global view (non-region)

**User** -> Usually a physical person
**Group** -> Functions (admin, devops) / Teams (engineering, design)
**Role** -> Internal usage within AWS resources

**Policies** -> Defines what each of the above can or can't do. Permissions are governed by Policies. `+` Least Privilege principle


### Security Groups

Network security in AWS. They control how traffic will be allowed into your EC2 Machines.

* Access to ports
* Authorised IP ranges - IPv4 and IPv6
* Control of inbound network
* Control of outbound network

* Can be attached to multiple instances
* Locked down to a region/VPC (Virtual Private Cloud) combination

> It's good to maintain one separate security group for SSH access

### Elastic IP

* Stop -> Start an instance EC2 -> Change the public IP
* Elastic IP -> Fixed public IPv4

> Avoid Elastic IP. Often reflect poor architectural decisions
> It's better use a random public IP and register a DNS name to it
> It's better use a Load Balancer and don't use a public IP

### Load Balancer Security Groups

* Load Balancer Security Group -> Source - 0.0.0.0/0 Allow HTTP from anywhere
* Application Security Group -> Source - sg-054b5ff5eaa02f2b6e Allow Traffic only from Load balancer