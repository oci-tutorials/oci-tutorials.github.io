---
date: 2019-01-25
title: Serverless Functions on OCI using FN
description: The Fn project is an open source, container native, and cloud agnostic serverless platform which is being funded by Oracle. It’s easy to use, supports every programming language, and in my opinion, performs very well.
categories:
  - Serverless
resources:
  - name: "Serverless Functions on OCI using the open source FN project"
    link: https://medium.com/@brianbmathews/getting-started-with-minikube-docker-container-images-for-testing-kubernetes-locally-on-mac-e39adb60bd41
    link: https://github.com/oci-tutorials
type: Tutorial
---

# Serverless Functions on Oracle Cloud using the open source FN project

![](https://cdn-images-1.medium.com/max/800/1*2KyQBP3r911sr6C3-Dh_Jg.png)
<span class="figcaption_hack">FN logo</span>

### Fn Project

The [Fn project](http://fnproject.io/) is an open source, container native, and
cloud agnostic serverless platform which is being funded by Oracle. It’s easy to
use, supports every programming language, and in my opinion, performs very well.

### Introduction

As this is an open source project as opposed to a lot of “as a service”
offerings available, we are left with the first task of, where we would like to
run the project.

The good thing about this, however, is that the FN project is very versatile so
you can run it almost anywhere from your laptop, to on-premise, to your
favourite cloud platform.

In this blog, I will be walking you through getting started with FN hosted on
Oracle Clouds OKE (Kubernetes Engine). There are 2 reasons which I chose this as
a platform for testing firstly the easy access and use of the OKE service for a
fully managed Kubernetes service and secondly its completely free to test with a
[free trial!](http://bit.ly/blogMedium)

### Getting started

For getting started with FN on Kubernetes the team at FN have made it really
easy by releasing a Helm chart to give easy installation. This chart deploys a
fully functioning instance of the [Fn](https://github.com/fnproject/fn) platform
on a Kubernetes cluster using the [Helm](https://helm.sh/) package manager.

### Prerequisites

* PV provisioner support in the underlying infrastructure (for persistent data,
see below )
* Install [Helm](https://github.com/kubernetes/helm#install)
* Initialise Helm by installing Tiller, the server portion of Helm, to your
Kubernetes cluster

#### Let get started:

Download helm using a package manager for ease:


Initialise Helm once installed:

    helm init

### Installing the Chart

Clone the fn-helm repo:

    git clone 
     && cd fn-helm

Install chart dependencies from
[requirements.yaml](https://github.com/fnproject/fn-helm/blob/master/fn/requirements.yaml):

    helm dep build fn

The default chart will install fn as a private service inside your cluster with
ephemeral storage, to configure a public endpoint and persistent storage you
should look at
[values.yaml](https://github.com/fnproject/fn-helm/blob/master/fn/values.yaml)
and modify the default settings. For OKE on Oracle Cloud it is as follows:

    fn:                         
    service:                            
    annotations:                               service.beta.kubernetes.io/oci-load-balancer-shape: 400Mbps                       ui:                         
    service:                            
    annotations:                                 service.beta.kubernetes.io/oci-load-balancer-shape: 400Mbps                                               mysql:                          
    persistence:                             
    enabled: true                             
    existingClaim: tc-fn-mysql                                               redis:                          
    persistence:                             
    enabled: true                             
    existingClaim: tc-fn-redis                                                                       rbac:                         
    enabled: true

Once you have this edited for your relevant Kubernetes host we can go ahead and
install the chart on Kubernetes.

To install the chart with the release name `release`:

    helm install --name release fn

**Note:*** if you do not pass the — name flag, a release name will be
auto-generated. You can view releases by running helm list (or helm ls, for
short).*

Now that FN has been installed we can start the service.

    fn start

The above command runs Fn in a single server mode with an embedded database and
queue. Behind the scenes, the `fn start` command runs a Docker image called
`fnproject/fnserver` in a privileged mode. It also mounts the Docker socket into
the container as well as the `/data` folder in the current working directory
(this is where database and queue information is stored). Finally, it exposes
port `8080` to the host so you can invoke it on that port.

### First function

The FN CLI which we have installed has a built-in `init` command which we use
for new functions.

For FN & Serverless functions there are a few terms which are useful to know
that I will refer too in the next steps.

**Apps** <br> Apps are a way to group your functions and triggers logically
under the same name e.g. `test-app`

**Triggers**<br> Triggers are pointers to functions used for invoking the
function code. Think of it as an endpoint where the function can be invoked
from, e.g.`http://localhost:8080/test-app/helloworld` which uses a HTTP trigger.
**NOTE: **You can have multiple triggers pointing to the same function, eg
events, HTTP triggers, scheduled, etc.

**Functions**This is the piece of code you are writing and that gets executed.

**Images**<br> Docker image that packages your function; the image used depends
on the language of the function (e.g.`fnproject/go`, `fnproject/ruby`), for the
best performance of functions we want our images to be as small as possible to
make the function easier to scale.

#### Let’s get started

Now that we have the terminology sorted let’s get started and make our sample
app.

To do this we use the `init` command to create a function with a Golang runtime
`--runtime go` then the provide it with the trigger type `--trigger http` and
then finally the subfolder for the function in this case `hello`

    fn init --runtime go --trigger http hello

When we go to the newly created function `hello` subfolder, in this folder we
can see the structure of the function, eg:

    hello
    ├── Gopkg.toml
    ├── func.go
    ├── func.yaml
    └── test.json

The source code for your function is in the `func.go` file and has a function
handler that responds with a *“Hello World”* message. The `func.yaml` file
contains information such as version runtime, name, entry point for your
function and a list of triggers.

You may look at the `test.json` file and wonder what it is, this file holds an
array of tests (input values and expected output values) that can be used to
test your function as a black-box service `fn test`. This is really useful
fortesting any services in the beta stage.

Now that we have created our function using the built-in `hello` example we can
run the function for testing using the `fn run` command.

**Note: **Before you run the function make sure you set the `FN_REGISTRY`
environment variable to your Docker repository.

Now when you run the command, Fn will build the Docker image with the function
and runs the function like this:

    fn run

    Building image hello:0.0.1 ...........
    {"message":"Hello World"}

This is all great, but we have the Fn server running locally, so let’s deploy
our function to the server, instead of just running it.

To deploy the function, you can use the `fn deploy` command and specify the app
name. Note that you need to run the command below from within the function
folder:

    fn deploy --app myapp

This command deploys the app (called `myapp`) and a function called `hello `to
the local Fn server and links a trigger called `hello-trigger` to that function.

This means that on the Fn server, the function will be accessible under
`/myapp/hello-trigger` path (e.g. `http://**ClsuterIP**/t/myapp/hello-trigger`).
The app name is used to logically group functions together. To see the full list
of triggers defined on the Fn server, just run this command:

    # List all triggers for 'myapp'
    fn list triggers myapp

    FUNCTION        NAME            TYPE    SOURCE          ENDPOINT
    hello           hello-trigger   http    /hello-trigger  

Finally, if you access the endpoint, you will get back the “*Hello World*”
message like this:

    curl 
    {"message":"Hello World"}

### Grouping functions

To group the functions together, we can use the `app` name construct — this
allows you to group different functions together logically (e.g. `test-app`
could have functions such as `hello` and `goodbye`).

In this case, the `test-app` could also be the folder where your functions live,
with their subfolders eg. `/hello` and `/goodbye `which contain the actual
functions. You can also define the `app.yaml` file in the app root folder, to be
able to deploy all functions with one command.

Follow the steps below to create a `test-app` with hello and goodbye functions:

    # Create the test-app folder
    mkdir test-app
    cd test-app

    # Create app.yaml that defines the app name
    echo "name: test-app" > app.yaml

    # Create a hello function in /hello subfolder
    fn init --runtime go --trigger http hello

    # Create a goodbye function in /goodbye subfolder
    fn init --runtime go --trigger http goodbye

Next, we can go into the `/hello` and the `/goodbye` folder, and deploy each app
separately with `fn deploy --app test-app`. Alternatively, since we have the app
name defined in `app.yaml` in the root folder, we can use this command to deploy
all functions to the Fn server:

    fn deploy --all

The above command creates the following functions and triggers:

    fn list triggers test-app

    FUNCTION        NAME            TYPE    SOURCE                  ENDPOINT
    goodbye         goodbye-trigger http    /goodbye-trigger        
    hello           hello-trigger   http    /hello-trigger          

You can also create a function that lives in the root of your app by running `fn
init` command from the apps root folder:

    fn init --runtime node --trigger http

Then deploy it again:

    fn deploy --all

Now we have three *triggers* defined under the *greeter-app* logical group:

    fn list triggers test-app

    FUNCTION        NAME                    TYPE    SOURCE                  ENDPOINT
    goodbye         goodbye-trigger         http    /goodbye-trigger        
    greeter-app     test-app-trigger     http    /greeter-app-trigger    
    hello           hello-trigger           http    /hello-trigger          

### Enabling the UI

If you prefer to interact with a UI, you can also do this with FN. Assuming you
have the Fn server running locally, you can start the UI l:

    docker run --rm -it --link fnserver:api -p 4000:4000 -e "FN_API_URL=
    " fnproject/ui

When the image has been downloaded and the container has been executed, you’ll
be able to access the UI on `http://localhost:4000`.

#### Try it yourself for free!

Now that you’ve seen how FN works and how to get started and host it on the
cloud, why not give it a go yourself with up to 3500 hours worth of free credits
on Oracle Cloud [http://bit.ly/brianMediumBlog](http://bit.ly/brianMediumBlog)

> Brian Mathews [@DevOps4Days](https://twitter.com/DevOps4Days)
