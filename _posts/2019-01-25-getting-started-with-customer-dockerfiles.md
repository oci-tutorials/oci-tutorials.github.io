---
date: 2019-01-25
title: Creating custom Dockerfiles for Node.js Function’s
description: The aim of this tutorial is to walk through how you can use a custom Docker image to define an Node.js serverless function.
categories:
  - Serverless
resources:
  - name: "Getting started with Custom Dockerfiles for Node.js Serverless Function’s"
    link: https://medium.com/devopslinks/building-custom-dockerfiles-for-node-js-serverless-functions-with-local-testing-using-fn-8462d5c5996f
  - name: "Source code"
    link: https://github.com/oci-tutorials
type: Tutorial
---

# Getting started with Custom Dockerfiles for Node.js for Serverless Function’s

![](https://cdn-images-1.medium.com/max/2560/1*Njm6sMNgKzhyVhPzwf7g0g.png)

#### **Intro:**

The aim of this tutorial is to walk through how you can use a custom Docker
image to define an Node.js serverless function.

For the purpose of this tutorial I used the opensource Fn project for testing as
you can run it on your local machine. For more information on Fn see tee the
following page [http://fnproject.io/](http://fnproject.io/) or take a look at my
last blog which walks you through configuring Fn to run on [Kubernetes on a free
OCI Cloud
trial](https://medium.com/@brianbmathews/going-serverless-on-oracle-cloud-using-the-open-source-fn-project-3c71f843b6d).

For the demo function I thought I would try something and use
[ImageMagick](https://www.imagemagick.org/) to do some nice simple image
processing in our function and while there is a Node.js module for ImageMagick,
it’s really just a wrapper on the underlying native library. So we’ll have to
install the library in addition to adding the Node module to our `package.json`
dependencies. Let’s start by creating the Node function

**Prequisites**

This tutorial requires you to have both Docker and Fn installed. If you need
help with Fn installation you can find instructions in the [Install and Start Fn
Tutorial](https://fnproject.io/tutorials/install/).

**Getting Started**

If it isn’t already running, you’ll need to start the Fn server. We’ll run it in
the foreground to let us see the server log messages so let’s open a new
terminal for this.

Start the Fn server using the fn cli:

    Fn start

### Function Definition

Firstly we shall create a folder for our deployment package:

    mkdir magick-function

We shall now jump into that folder:

    cd magick-function

In an **empty folder** create a file named `func.js`

    nano func.js

In this function you can paste the following as its content:

    const fdk = require(‘@fnproject/fdk’);

    const fs = require(‘fs’);

    const tmp = require(‘tmp’);

    const im = require(‘imagemagick’);

    fdk.handle((buffer, ctx) => {

    return new Promise((resolve, reject) => {

    tmp.tmpName((err, tmpFile) => {

    if (err) throw err;

    fs.writeFile(tmpFile, buffer, (err) => {

    if (err) throw err;

    im.identify([‘-format’, ‘{“width”: %w, “height”: %h}’, tmpFile],

    (err, output) => {

    if (err) {

    reject(err);

    } else {

    resolve(JSON.parse(output));

    }

    }

    );

    });

    });

    });

    }, { inputMode: ‘buffer’ });

This function is pretty straight forward, it takes a binary image as it’s
argument, writes it to a tmp file, and then uses ImageMagick to obtain the width
and height of the image. Since the function argument type is binary we need to
set the “inputMode” property to “buffer” when we call the the FDK’s handle
function.

### Declaring Node.js Dependencies

There are some interesting elements to this function, but the key one for us is
the use of the “imagemagick” Node module for image processing.

To use it we need to include it in our dependencies in the `package.json` along
with the other dependencies.

In same folder as the `func.js` file, create a `package.json` file and paste the
following as its content:

    {

    "name": "imagedims",

    "version": "1.0.0",

    "description": "Function using ImageMagick that returns dimensions",

    "main": "func.js",

    "author": "fnproject.io",

    "license": "Apache-2.0",

    "dependencies": {

    "@fnproject/fdk": ">=0.0.11",

    "tmp": "^0.0.33",

    "imagemagick": "^0.1.3"

    }

    }

Like all Node.js functions using the Fn Node FDK we include it as a dependency
along with the “tmp” module which as I mentioned we need for the temporary file
utilities and “imagemagick” for image processing.

### Function Metadata

Now that we have a Node.js function and it’s dependencies captured in the
`package.json` we need a `func.yaml` to capture the function metadata.

In the folder containing the previously created files, create a `func.yaml` file
and paste the following as its content:

    schema_version:
    20180708

    name:
    imagedims

    version:
    0.0.1

    runtime:
    docker

    triggers:

    -
    name:
    imagedims-trigger

    type:
    http

    source:
    /imagedims

**Note: **This is a pretty typical `func.yaml` for a Node.js function,
**except** that instead of declaring the **runtime** as “node” we’ve specified
“**docker**”. If you were to type `fn build` right now you’d get the error:

    $ Fn: Dockerfile does not exist for ‘docker’ runtime

This is because when you set the runtime type to “docker” `fn build` defers to
your Dockerfile to build the function container image–and we haven’t defined
this yet.

### Default Node.js Function Dockerfile

The Dockerfile that `fn build` would normally generate to build a Node.js
function container image looks like this:

FROM fnproject/node:dev as build-stage
WORKDIR /function
ADD package.json /function/
RUN npm install
FROM fnproject/node
WORKDIR /function
ADD . /function/
COPY --from=build-stage /function/node_modules/ /function/node_modules/
ENTRYPOINT ["node", "func.js"]

It’s a two stage build with the `fnproject/node:dev` image containing `npm` and
other build tools, and the `fnproject/node` image containing just the Node
runtime. This approach is designed to ensure that deployable function container
images are as small as possible–which is beneficial for a number of reasons.

### Custom Node.js Function Dockerfile

The `fnproject/node` container image is built on Alpine so we’ll need to install
the ImageMagick Alpine package using the `apk` package management utility. You
can do this with a Dockerfile `RUN` command:


We want to install ImageMagick into the runtime image, not the build image, so
we need to add the `RUN` command after the `FROM fnproject/node` command.

In the folder containing the previously created files, create a file named
`Dockerfile` and paste the following as its content:

FROM fnproject/node:dev as build-stage
WORKDIR /function
ADD package.json /function/fn
RUN npm install
FROM fnproject/node
RUN apk add --no-cache imagemagick
WORKDIR /function
ADD . /function/
COPY --from=build-stage /function/node_modules/ /function/node_modules/
ENTRYPOINT ["node", "func.js"]


With this Dockerfile, the Node.js function, it’s dependencies (including the
“imagemagick” wrapper), and the “imagemagick” Alpine package will be included in
an image derived from the base `fnproject/node` image. We should be good to go!

Now we can build our file:

### Building and Deploying

Once you have your custom Dockerfile you can simply use `fn build` to build your
function. Give it a try:

`fn -v build`

You should see output similar to:



Just like with a default build, the output is a container image. From this point
forward everything is just as it would be for any function. Since I previously
started an Fn server, you can deploy it now and test. Let’s deploy to an
application named ‘tutorial’:

`fn deploy --app tutorial --local`

We can confirm the function is correctly defined by getting a list of the
functions in the “tutorial” application:

`fn list functions tutorial`

**Tip**: The fn cli let’s you abbreviate most of the keywords so you can also
say `fn ls f tutorial`

You should see output similar to:


### Invoking the Function

With the function deployed let’s invoke it to make sure it’s working as
expected. You’ll need a jpeg or png file so either find one on your machine or
download one. I used a random photo on my laptop

`cat Test-image.jpg | fn invoke tutorial imagedims`

For this file you should see the following output:

`{"width":720,"height":540}`

![](https://media.giphy.com/media/26ufdipQqU2lhNA4g/giphy.gif)

### Calling the Function with curl

You may have noticed when creating the `func.yaml`we included a HTTP trigger
declaration, this is so we can also call the function easily with curl.

It’s a little more complicated as you need to declare the content type because
the request body is binary. You also need to use the `--data-binary` switch:

`curl --data-binary Test-image.jpg -H "Content-Type: application/octet-stream"
-X POST http://localhost:8080/t/tutorial/imagedims`

You should get exactly the same output as when using `fn invoke`.

### Conclusion

One of the most powerful features of Fn is the ability to use custom defined
Docker container images as functions. This feature makes it possible to
customize your function’s runtime environment including letting you install any
Linux libraries or utilities that your function might need. And thanks to the Fn
CLI’s support for Dockerfiles it’s the same user experience as when developing
any function.

Having completed this tutorial you’ve successfully built a function using a
custom Dockerfile. Well done, why not give it a go now with Fn hosted on the
Kubernetes on OCI for free as outlined in [my previous
blog](https://medium.com/@brianbmathews/going-serverless-on-oracle-cloud-using-the-open-source-fn-project-3c71f843b6d)!

> Brian Mathews [@DevOps4Days](https://twitter.com/DevOps4Days)
