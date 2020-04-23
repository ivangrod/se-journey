+++
title = "AWS - EC2"
date = 2020-04-19T10:49:11+02:00
weight = 1
+++

* Launch virtual machines on the cloud
* Storing data on virtual devices (EBS)
* Distributing load across machines (ELB)
* Scaling the services using an auto-scaling group (ASG)

> **AMI**: Amazon Machine Image. An image to use to create our instances. They're built for a specific AWS region.

### EC2 User Data

AWS comnes with base images which can be customized using EC2 User Data:

Bootstrapping in an EC2 instance with EC2 user data script. Only run once <-> First start.

It's used to automated boot tasks:
* Installing updates
* Installing software
* Downloading common files

## EC2 Instance Launch Types

* **On Demand**: Short workloads. Pay for what you use.

Recommend for short-term and un-interrupted workloads, where you can't predict how the application will behave.

* **Reserved**: Long workloads (>= 1 year). Pay upfront for what you use with long term commitment

Recommend if you have a database and you know it's going to be steady for a year or three years

* **Convertible reserved**: Long workloads with flexible instances. Insecure about needing a C4, C4 large

* **Scheduled reserved**: Reserve capacity, but only for a small window.
* **Spot**: Short workloads. Very cheap. Loose instances.

Recommend for batch jobs, Big data analysis or workloads that are resilient to failures.

* **Dedicated**: No other customer will share our underlying hardware. Visibility into the underlying sockets / physical cores of the hardware.
* **Dedicated Hosts**: Book the entire physical server

https://www.ec2instances.info/ There are five distints characteristics:

* RAM (type, amount, generation)
* CPU (type, frequency, generation, number of cores)
* I/O (disk performance, ESB optimisations)
* Network (network bandwidth, network latency)
* GPU (Graphical Processing Unit)

> T2/T3 are **burstable** instances. Can be amazing to handl unexpected traffic and getting the insurance that it will be handled correctly.