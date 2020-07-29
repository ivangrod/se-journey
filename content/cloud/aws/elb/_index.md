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
* Application Load Balancer (v2 - new generation) -> HTTP (HTTP2), HTTPS, Websocket
* Network Load Balancer (v2 - new generation) -> TCP, TLS, UDP

### Application Load Balancer ++

**Routing**

* Routing tables to different target groups
  * Based on path in URL
  * Based on hostname in URL
  * Based on Query String, Headers 

> ALB are great fit for microservices & container-based application

**Target Groups**

* EC2 instances - HTTP
* ECS tasks - HTTP
* Lambda functions - HTTP request is translated into a JSON event
* IP Addresses - private IPs

**GTK (Good to Know)**

* Fixed hostname with your Application Balancer
* The application servers don't see the IP of the client directly.
  * The true IP of the client is going to be inserted in the header called **X-Forwarded-For**.
  * You can also get the Port using **X-Forwarded-Port** and the protocol being used using **X-Forwarded-Proto**.


## Load Balancer Stickiness

Stickiness -> The same client will always be directed to the same underlying instance that is behind the load balancer. Classic Load Balancer and Application Load Balancer.

The **cookie** used for stickiness has an expiration date you control.

> Use Case: Make sure the user doesn't lose his session data

¡¡¡ Enabling stickiness may bring imbalance to the load over the backend EC2 instance.

## Cross-Zone Load Balancing

When **cross-zone** balancing is enabled, that means that each load balancer instance will evenly distribute the traffic across all the instances in all AZ.

## SSL Certificates

* Traffic between your clients and your load balancer to be encrypted in transit (in-flight encryption)

> TLS (Transport Layer Security) > SSL (Secure Socket Layer)

* The load balancer uses an X.509 certificate (SSL/TLS certificate)
* **ACM (AWS Certificate Manager)**
* SNI (Server Name Indication). It solves a very important problem, which is how do you load multiple SSL certificates onto one web server, in order for that web server to serve multiple websites.

## Connection Draining

1. Time to complete "in-flight requests" while the instance is de-registering or unhealthy
2. Stops sending new requests to the instance which is de-registering

:) Set a low value if your request are short (300 seconds by default)