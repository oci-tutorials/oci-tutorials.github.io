---
date: 2019-03-26
title: Getting started with Drupal 8 on Kubernetes
description: This is tutorial to deploy a new Drupal Website on a Kubernetes cluster hosted on Oracle Container Engine (OKE)
categories:
  - Kubernetes
resources:
  - name: "What is OCI?"
    link: https://cloud.oracle.com/en_US/iaas/getting-started
  - name: "Oracle Developer Community"
    link: https://developers.oracle.com
  - name: "Source code"
    link: https://github.com/oci-tutorials
type: Post
---

### Getting started with Drupal 8 on Kubernetes

![](https://cdn-images-1.medium.com/max/800/1*41nd0BgxHW_zrFgrovfndA.png)
<span class="figcaption_hack">Drupal 8 on Kubernetes</span>

### Introduction

In this blog post, I am going to leverage on the experience I have gained in my
previous role — where I was managing the infrastructure and application layer
for a large Drupal platform in a multi-site architecture, and my current role as
Cloud Solution Engineer at Oracle.

[Drupal is an open-source Content Management System (CMS)](http://drupal.org)
that is supported by a very large community, contributing (often for free) to
improve the product and enrich it with very useful functionalities.

The idea is to use [Oracle Container Engine
(OKE)](https://cloud.oracle.com/containers/kubernetes-engine) and Helm to
install a [Drupal 8](http://drupal.org) Chart from
[Bitnami](https://bitnami.com/stack/drupal/helm) on a Kubernetes cluster running
in [Oracle Cloud](http://bit.ly/LucaMediumBlogDrupal). This allows developers to
have a Cloud native Drupal development environment in seconds for testing new
modules, themes, or just testing bug fixes.

### Prerequisites

As we are deploying a Drupal 8 website on a Kubernetes cluster, the most
important prerequisite is…to have a Kubernetes cluster! Make sure to follow the
[previous tutorial I wrote to create a new
cluster](https://medium.com/devopslinks/kubernetes-deploying-a-cluster-with-a-few-clicks-using-a-web-console-5dbf75abc853),
install and configure `kubectl`. In addition, we need to install `helm.`

![](https://cdn-images-1.medium.com/max/1200/1*IfBwEOHabj24NO-kGc3HNA.gif)
<span class="figcaption_hack">Kubernetes cluster created in Oracle Container Engine (OKE)</span>

#### Installing and configuring Helm

[Helm](https://helm.sh/) is like a *“packet manager”* for Kubernetes. It allows
to define, install, and configure even the most complex applications as Charts.
Charts can be easily deployed on a Kubernetes cluster, where Tiller (the Helm
server-side component) is running, with a few simple commands.

There are several ways to install Helm on your machine and it depends mainly on
which is your OS. I have used the following command on my Windows 10 machine
using [Git Bash](https://git-scm.com/downloads):

    curl https://raw.githubusercontent.com/helm/helm/master/scripts/get | bash

After a successful installation, we can simply initialize it using `helm init
--upgrade`. The `--upgrade` flag will make sure to initialize Tiller running in
the Kubernetes cluster too.

You will receive a confirmation message similar to the below:

    $HELM_HOME has been configured at /home/oracle/.helm.

    Tiller (the Helm server-side component) has been upgraded to the current version.
    Happy Helming!

We can also verify Tiller is running in its own Pod by typing `kubectl get pods
--namespace kube-system`:

![](https://cdn-images-1.medium.com/max/800/1*iNisAn3RL7b_orgPmxOFKg.png)
<span class="figcaption_hack">Tiller is running in our Kubernetes cluster</span>

### Deploying Drupal Helm Chart

Now that we have Helm (and Tiller) installed and configured in our local machine
and Kubernetes cluster, we can actually deploy the Drupal Helm Chart from
Bitnami — the Chart source code can be accessed
[here](https://github.com/bitnami/charts/tree/master/upstreamed/drupal).

Deploying a new Drupal 8 website is as easy as running the following two
commands:

    # Update helm charts repositories (optional, but recommended)
    helm repo update

    # Deploy the Drupal stable chart as "d8cluster"
    helm install stable/drupal --name d8cluster

Helm will process the Chart and its dependencies, e.g. MariaDB as Drupal backend
database, and create all the components in our Kubernetes cluster as required.

Using the default `values.yaml` configuration file, the following main
components will be created in our cluster — as you can see from the image below:

![](https://cdn-images-1.medium.com/max/800/1*IcHPbTNYolmxuFTANcL5HA.png)
<span class="figcaption_hack">Helm will output a short description for all the components created in the
cluster</span>

1.  A *ConfigMap* that contains the MariaDB configuration variables
1.  Two *PersistentVolumeClain* to allocate Block Storage volumes to persist Apache
configuration and Drupal assets
1.  Two *Pods*: one for Drupal and one for MariaDB. These will run in the actual
cluster nodes created by OKE
1.  Two *Secrets*: these will contain the credentials for the Drupal admin user
(needed to login in the site later) and the database admin user
1.  Two *Services*: this is an interesting topic. As you can see, `d8cluster-drupal`
is a `LoadBalancer` type, which will result in a native Load Balancer created in
the Oracle Cloud Infrastructure, exposing ports 80 and 443 with a Public IP that
can be accessed from the public Internet. This will be the entry point for our
website and will dispatch the traffic to the Drupal pod in a private subnet
accordingly. The other Service, `d8cluster-mariadb`, is a `ClusterIP` type. In
this case, it has a “private” Cluster IP that can be accessed by the cluster
pods only and it’s not publicly exposed. **It is important to mention that OKE
will also create and manage the necessary Security rules to allow traffic
between the Load Balancer and the Pods.** This can be also verified by
inspecting the Security Lists on Oracle Cloud Infrastructure Web console
1.  A *Deployment*: this represents the Drupal application itself
1.  A *StatefulSet*: this is used to manage the lifecycle and consistency of the
MariaDB pods in case we want to scale up/down this component

After a few minutes, the new Drupal 8 website will be ready to be accessed using
the Load Balancer Public IP.

### Accessing the Drupal 8 website

As mentioned in the previous paragraph, we need to retrieve the Load Balancer
Public IP. We can do it in several ways but the easiest way is to run the
following command:

    # Retrieve the Load Balancer Public IP
    kubectl get svc --namespace default d8cluster-drupal

The output will be something like:

![](https://cdn-images-1.medium.com/max/800/1*llRGyGTeQsXQ__uSdWcbvg.png)
<span class="figcaption_hack">Take note of the Load Balancer Public IP</span>

The external IP can be entered in a browser to access the website:

![](https://cdn-images-1.medium.com/max/1200/1*0Nxc31b8GbLVhA1HaAgUbg.png)
<span class="figcaption_hack">Here is our brand new Drupal 8 site running on Kubernetes</span>

### Login as website Admin

The website Admin credentials have been created automatically during the Chart
deployment.

We can use the following command to retrieve the password and base64-decode it:

    kubectl get secret --namespace default d8cluster-drupal -o jsonpath="{.data.drupal-password}" | base64 --decode

Now we can head to the usual Drupal login page, e.g. “/user/login” and log in
using “user” as username and the password just retrieved:

![](https://cdn-images-1.medium.com/max/1200/1*o6BrAkuF08kxIX0oyT39DA.gif)
<span class="figcaption_hack">Login as Admin using the generated credentials</span>

### Accessing Drupal source code

After being able to access the Drupal website from the browser, you might be
interested to access the source code and edit it for development purposes. If
your Kubernetes nodes are in a private subnet, you need to create a *bastion
host *in a public subnet and use it to SSH into the cluster node as described in
this [whitepaper](https://cloud.oracle.com/iaas/whitepapers/bastion_hosts.pdf).

Within the cluster node, it is possible to mount the Block Storage Volume that
has been attached by OKE during the Chart deployment, e.g.:

    # Replace sdX with the actual device listed using fdisk -l
    sudo mount /dev/sdX /bitnami/drupal

If we `cd /bitnami/drupal`, we will recognize the usual Drupal site folder
structure and can use our favorite IDE to start coding:

![](https://cdn-images-1.medium.com/max/1200/1*5ZGv72cjE-8B0PHD2Wd5Rg.png)
<span class="figcaption_hack">A usual Drupal site folder structure</span>

### Next steps

Now that we have a new Drupal site up and running, my goal is to secure it using
a Web Application Firewall (WAF). In the next blog post, I will demonstrate how
to use the [recently introduced Oracle Cloud
WAF](https://blogs.oracle.com/cloud-infrastructure/introducing-the-oci-waf) to
protect our Drupal website.

### Try it for free on Oracle Cloud

OKE is only one of the great services offered by Oracle Cloud. There are many
others that can be explored to enhance your cloud-native applications.

You can try the steps I described in this tutorial by [registering a free trial
account here](http://bit.ly/LucaMediumBlogDrupal), and take the opportunity to
discover many other services offered by OCI.

[Try Oracle Cloud for 30 days!](http://bit.ly/LucaMediumBlogDrupal)


***

> Luca Iannario [@liannario](https://twitter.com/liannario)
>
> Developer Advocate at Oracle. 
> When I am not on the clouds, I like travelling and taking pictures.
