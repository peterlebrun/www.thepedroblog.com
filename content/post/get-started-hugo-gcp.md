+++
title = "Getting Started with Hugo and GCP"
author = "Pedro"
date = "2018-08-12"
+++

## What is Hugo?
Hugo is a static site generator.  A few days ago I would have said "a blog? I should write my own framework!"  But then I got stuck trying to do CSS.  And I didn't feel like administering a database or a web server.  Turns out that static site generators eliminate most of the hassle of getting good content on the web, reduce your security footprint, provide great scalability (this blog, for instance, is backed by google infrastructure), and can provide insanely fast deployments as well.

## What will you learn today?
Today I'll walk you through setting up a blog with Hugo, hosted on Google Cloud Storage.  This tutorial is aimed at someone with technical skills, comfortable with markdown and interested in learning how to use google cloud.  There are a couple basic steps:
1. Set up Hugo, create a blog, and push remote
2. Set up a Cloud Storage bucket
3. Set up a Cloud Container build trigger
4. Set up a domain

## What you need
To get the most out of this tutorial, you'll need:
- A github account (You can also use cloud source repository or Bitbucket)
- A google cloud account
- (obviously) access to a computer.  Here I'll be using a Mac.  If you are using a different OS, you'll need to tailor the commands to your OS.

## Step 1: Set up Hugo
- First, install hugo:
`
brew install hugo
`
- Next, create a hugo blog:
`
hugo new site quickstart
cd quickstart
`
- Now, create a post!
`
hugo new post/first.md
hugo server -D
`
- Running `hugo server -D` will start a development server and tell you how to view it (mine lives at localhost:1313)
- Next, install a theme (choose your theme from [xyz](https://www.google.com)):
```
echo "themes/*" >> .gitignore
git clone bar
cp ./* layouts
```
- Turn this into a git repo `git init`
- Set up a remote
`git remote add origin {YOUR_REPO_URL}`
- Push!
`git push -u origin master`

## Step 2: Set up a domain
- Go to [Google Domains](https://domains.google) and register a new domain. (Here we'll use www.example.com)
- Set up a CNAME record for `www` pointing to `c.storage.googleapis.com.`
(image here)

## Step 3: Set up a Cloud Storage bucket
- Create a cloud storage bucket with the name of your domain and CNAME: www.example.com
(image here)
`gsutil mb gs://www.example.com`
- Note that you need to use a domain that you own and google has verified that you own, otherwise you'll get a 403 error.

## Step 4: Get a Hugo container available for your use
- Build a Hugo container and push it to your cloud container registry.  See [here](https://github.com/GoogleCloudPlatform/cloud-builders-community/blob/master/hugo/Dockerfile) for a sample Dockerfile that I used.  Make sure you update the hugo version in the Dockerfile to match your local hugo version (`hugo version`).
`docker build hugo-docker .`
`docker tag hugo-docker gcr.io/{YOUR_PROJECT_ID}/hugo`
`docker push gcr.io/{YOUR_PROJECT_ID}/hugo`

## Step 5: Create your cloudbuild.yaml
- Create a file in the root of your blog directory called cloudbuild.yaml
`touch cloudbuild.yaml`
- Put the following content into your cloudbuild.yaml file:
```
steps:
- name: gcr.io/cloud-builders/git
  args: ['clone', 'https://github.com/EmielH/tale-hugo.git', 'themes/tale']
- name: gcr.io/{YOUR_PROJECT_ID}/hugo
- name: gcr.io/cloud-builders/gsutil
  args: ['-m', 'rsync' , '-r' , '-c', '-d', './public', 'gs://www.thepedroblog.com']
```