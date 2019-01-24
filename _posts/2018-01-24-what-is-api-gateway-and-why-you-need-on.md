---
date: 2018-01-24
title: What is API gateway and why you need one?
description: Introduction about API gateways. What it is and why you need one. 
categories:
  - Integration
resources:
  - name: "What is Oracle API Platform?"
    link: https://cloud.oracle.com/en_US/api-platform
  - name: "Oracle Developer Community"
    link: https://developers.oracle.com
  - name: "Source code"
    link: https://github.com/oci-tutorials
type: Tutorial
---

We can say APIs are everywhere nowadays. Likely any app you build now consumes data from other apps, your own or from 3rd parties. That means you need to expose your app services to the world as well.

![The puzzle of integrating APIs](https://images.unsplash.com/photo-1525857222756-37cdb4e87e36?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2850&q=80)

## Why I need an API gateway?

How many requests are you expecting per second? How many do you want to allow per minute? What happens if those requests are invalid? What if they are dangerous? Do you need authentication? What about authorization? I’m sure you want TLS on, right?

Let’s assume you know the answer to those questions. Now it’s time to implement all of those validations, rate limiting, auto-scaling, filtering out bad requests, integrate with OAuth, maybe OpenID Connect, TLS termination, and so on.

Aren’t all those cross-cutting aspects? Do we want to pollute our business code with that extra noise? The approach to these concerns should be the same across all contributors and teammates implementing the API.

Let’s be honest, it is not rocket science but as soon as we start implementing these new features, someone is going ask for configuration parameters and visibility of the process. At that point, it might be out of hand.

Some of the APIs endpoints would be public, other private. That would change and someone has to implement and enforce those policies. If you are building layers of microservices to provide internal and external services for other apps. It’s getting complicated.

## Let’s try an API gateway

What if we centralize the APIs of all your apps in a single place where you can configure non-functional requirements and manage policies, handle all the request per second and limit them to a threshold. Monitoring the result. The same way a reverse proxy works the API gateway can redirect the requests to the correct app inside your infrastructure. Imagine that you can handle TLS termination there to simplify the logic in your code. You don’t need to handle all the authentication and authorization in every app independently.

The good news is that API Gateway is exactly what you need. Manage hundreds of APIs from different systems in a centralized place. Even for a single API I would recommend it, you gain some insights about what the users are doing with your APIs and stop attacks in a lot of scenarios.

Even more, after a few months your API is getting traction, maybe you can implement some extra features for those that are willing to pay for some advanced features. API gateway can give you an easy way to analyze the requests and even monetize those fancy new ideas.

## Trade-offs

Now that you are happy bringing in a new API gateway, you have to make sure the routing is correct at deployment time. You don’t want to have a call because some user can reach out your API.

Your API gateway should have the capacity to handle all the requests and be resilient. Think about what infrastructure is needed. Load balancer, metrics and alerts, and so on. Keep in mind that the complexity doesn’t disappear, it is moved to a place where you can manage more easily.

## Try in the cloud

Some services in the cloud are available to handle all of this complexity for you. If you want to explore some options, feel free to play around with API platform cloud. It’s part of the catalog of cloud services that Oracle offers. See more on [API Platform](https://cloud.oracle.com/en_US/api-platform).

Create a [free trial account](https://cloud.oracle.com/tryit)!

Happy hacking!

***

> Victor Martin [@victorilloleon](https://twitter.com/victorilloleon)
>
> Software engineer and Oracle developer advocate.
> Long fancy name for just a guy who enjoys writing code and see it running in the cloud.