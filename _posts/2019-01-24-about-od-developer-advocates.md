---
date: 2019-01-24
title: Introduction to OD Developer Advocates
video_id: 19B6Sc5iSyA
description: A quick introduction the new advocacy team will focus on everything Cloud Native and OCI. 
categories:
  - Miscellanous
resources:
  - name: "What is OCI?"
    link: https://cloud.oracle.com/en_US/iaas/getting-started
  - name: "Oracle Developer Community"
    link: https://developers.oracle.com
  - name: "Source code"
    link: https://github.com/oci-tutorials
type: Video
set: about-us
set_order: 4
---

##About Us

Many people don’t use Jekyll for client projects as non-developers would traditionally have to learn HTML, Markdown and Liquid to update content. In this tutorial, we give non-developers an easy way to update Jekyll sites with [CloudCannon](https://cloudcannon.com).

## Why we are?

CloudCannon is cloud content management system and hosting provider for Jekyll websites. A developer uploads a Jekyll site in the browser or by syncing with GitHub, Bitbucket or Dropbox. CloudCannon then builds the site, hosts it and provides an interface for non-technical users to update content.

## How we can help?

To begin, we need to create a CloudCannon account and create our first site. Head over to [CloudCannon](https://cloudcannon.com) and click the *Get Started Free* button:

Enter your details into the sign up form:

Once we've signed up we're taken to our dashboard. Click *Create Site*:

Enter a name for the site. I'm going to use the site from the [Converting a static site to Jekyll](/jekyll-casts/converting-a-static-site-to-jekyll/) cast so I'll call it *Creative*:

This creates the site and gives us options for uploading our files. If you'd like to use the same site I'm using you can download it [here](https://github.com/CloudCannon/creative-jekyll-theme/archive/master.zip).

There's a number of ways of getting your files on CloudCannon. To keep things simple we're just going to upload a folder from our local computer. Click on the folder icon. *Note: folder upload is only supported in Chrome*

Navigate to your Jekyll site and click *Upload*:

Once the files upload, CloudCannon builds the site:

We can view the live site by clicking on the _.cloudvent.net_ URL in the sidebar:

## Coming Soon

Next, we need to do is to define areas in our HTML which non-developers can update. These are called [Editable Regions](https://docs.cloudcannon.com/editing/editable-regions/) and are set by adding a class of `editable` to HTML elements.

Open `index.html` in CloudCannon and add a class of `editable` to the `h1` and `p` inside `<div class="header-content-inner">` so it becomes the following:

~~~ html
<div class="header-content-inner">
  <h1 class="editable">Your Favorite Source of Free Bootstrap Themes</h1>
  <hr>
  <p class="editable">Start Bootstrap can help you build better websites using the Bootstrap CSS framework! Just download your template and start going, no strings attached!</p>
  <a href="/about.html" class="btn btn-primary btn-xl page-scroll">Find Out More</a>
</div>
~~~

If you are as excited as we and can't wait to get your hands on OCI, you can give it a try here: [Sign up free](https://app.cloudcannon.com/users/sign_up) and get $300 Free Credit. 
