+++
title = "AWS - ELB"
date = 2020-07-27T10:49:11+02:00
weight = 1
+++

Load balancers are servers that forward internet traffic to multiple servers (EC2 Instances) downstream.

* Spread load across multiple downstream instances
* Expose a single point of access (DNS) to your application.
* Seamlessly handle failures of downstream instances through health checks
* Do regular health checks to your instances.
* Provide SSL termination (HTTPS) or HTTPS security, for your website directly on load balancer site,
* Enforce stickiness with cookies.
* High availability across availability zones.
* Separate public traffic from private traffic
 

## ELB (EC2 Load Balancer)

Types of load balancer on AWS

* Classic Load Balancer (v1 - old generation) -> HTTP, HTTPS TCP
* Application Load Balancer (v2 - new generation) -> HTTP, HTTPS, Websocket
* Network Load Balancer (v2 - new generation) -> TCP, TLS, UDP
