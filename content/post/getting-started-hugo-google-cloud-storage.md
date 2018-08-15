+++
title = "Getting Started with Hugo and Google Cloud Storage"
author = "Pedro"
date = 2018-08-12
+++

This three part series will show you how to create and host your own blog using Hugo (my new favorite static site generator), Google Cloud Storage (at a cost of pennies per month!), and Google Cloud Build.  (This is how I set up and am running my own site.)  The only real cost will be your domain name.  There's a bunch more you can do with it, but here's enough to get started!

- <a href="{{< ref "create-hugo-blog.md" >}}">Part 1: Create your own Hugo Blog</a>
- <a href="{{< ref "create-cloud-storage.md" >}}">Part 2: Set up your Cloud Storage bucket</a>
- <a href="{{< ref "deploy-hugo-cloud-storage.md" >}}">Part 3: Set up Cloud Build</a>

## What is Hugo?
Hugo is a static site generator.  A few days ago I would have said "a blog? I should write my own framework!"  But then I got stuck trying to do CSS.  And I didn't feel like administering a database or a web server.  Turns out that static site generators eliminate most of the hassle of getting good content on the web, reduce your security footprint, provide great scalability (this blog, for instance, is backed by google infrastructure), and can provide insanely fast deployments as well.


## What you need
To get the most out of this tutorial, you'll need:
- A github account (You can also use cloud source repository or Bitbucket)
- A google cloud account
- (obviously) access to a computer.  Here I'll be using a Mac.  If you are using a different OS, you'll need to tailor the commands to your OS.


## Other things you can do:
- Set up an about page
`hugo new about.md`
- [Learn more about Hugo](https://gohugo.io/)
- [Set up SSL](https://cloud.google.com/storage/docs/troubleshooting#https)
- [Learn more about Cloud Build](https://cloud.google.com/cloud-build/)
- [Learn more about Cloud Storage](https://cloud.google.com/storage/)

Go nuts!  Have fun!