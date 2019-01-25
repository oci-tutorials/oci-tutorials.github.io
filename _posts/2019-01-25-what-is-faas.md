---
date: 2019-01-25
title: What is this “FaaS”?
description: This tutorial uses [Minikube](https://github.com/kubernetes/minikube) to createa local kubernetes cluster. This tutorial uses [Docker for Mac](https://docs.docker.com/engine/installation/mac/) as the host of Minikube.
categories:
  - Serverless
resources:
  - name: "What is 'Faas'?"
    link: https://medium.com/@brianbmathews/what-is-this-faas-thing-everyone-talking-about-aba5d5e8ebfa
  - name: "Source code"
    link: https://github.com/oci-tutorials
type: Tutorial
set: Serverless
set_order: 3
---

# What is this “FaaS” thing everyone talking about? I don’t want a “microservice”, I want a big service that makes me lots of money!

### What is Function-as-a-Service (FaaS)?

FaaS is the concept of serverless computing with serverless architectures.
Software developers can use this to deploy an individual function (Piece of
code), that performs an action, or piece of business logic, without worrying
about where they are running it (eg. serverless). They are expected to start
within milliseconds and process individual requests and then the process ends.
Sounds simple right?

**Principles of FaaS:**

* Completely takes the pain of servers and environment constraints away from the
developer
* Billing based on consumption and executions, not server instance sizes
* Services that are event-driven and instantaneously scalable

![](https://cdn-images-1.medium.com/max/800/0*5B_ycnAD2U7zITih.png)

At the basic level, you could describe them as a way to run some code when a
“thing” happens. Shows how easy it is to process an HTTP request as a
“Function”.


*****

#### Benefits & Use Cases

Like most things, FaaS is not going to be perfect for every app.

In general, we see companies and developers using them mostly for our very high
volume transactions so that they can scale when needed and don’t have to have
redundant servers the rest of the time.

* High volume transactions — Isolate them and scale them
* Dynamic or burstable workloads — If you only run something once a day or month,
no need to pay for a server 24/7/365
* Scheduled tasks — They are a perfect way to run a certain piece of code on a
schedule, think cron jobs.

*****

#### Types of Functions

There are a lot of potential uses for functions. Below is a simple list of some
common scenarios. Support and implementation for them vary by the provider or
service your using.

* Scheduled tasks or jobs.
* Process a web request.
* Process queue messages.
* Run manually.

*****

### Summary

Developers hate servers and server management. The idea of serverless
architectures is a dream came true for most developers. That said, I can’t see
FaaS as being a complete replacement for normal application architectures. For
example a basic web application, it would take a lot of functions.

In my humble opinion, function-based apps are a perfect fit for replacing
microservice style architectures and smaller high volume back-end services.

* [Serverless](https://medium.com/tag/serverless?source=post)
* [Cloud Computing](https://medium.com/tag/cloud-computing?source=post)
* [Developer](https://medium.com/tag/developer?source=post)
* [Programming](https://medium.com/tag/programming?source=post)
* [Introduction](https://medium.com/tag/introduction?source=post)


> Brian Mathews [@DevOps4Days](https://twitter.com/DevOps4Days)
