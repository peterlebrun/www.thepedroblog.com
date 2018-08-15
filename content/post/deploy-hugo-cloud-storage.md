+++
title = "Deploy Hugo Cloud Storage"
date = 2018-08-15
draft = false
+++
## What will you learn today?
Today I'll walk you through automating the deployment of your Hugo blog, so that your static site will build every time master changes.

By the end of this tutorial, you will be able to update your blog by pushing a markdown file to master.

## Get a Hugo container available for your use
- Build a Hugo container and push it to your cloud container registry.  See [here](https://github.com/GoogleCloudPlatform/cloud-builders-community/blob/master/hugo/Dockerfile) for a sample Dockerfile that I used.  Make sure you update the hugo version in the Dockerfile to match your local hugo version (`hugo version`).
`docker build hugo-docker .`
`docker tag hugo-docker gcr.io/{YOUR_PROJECT_ID}/hugo`
`docker push gcr.io/{YOUR_PROJECT_ID}/hugo`

## Create your cloudbuild.yaml
Create a file in the root of your blog directory called cloudbuild.yaml
```
touch cloudbuild.yaml
```
Put the following content into your cloudbuild.yaml file:
```
steps:
- name: gcr.io/cloud-builders/git
  args: ['clone', 'https://github.com/EmielH/tale-hugo.git', 'themes/tale']
- name: gcr.io/{YOUR_PROJECT_ID}/hugo
- name: gcr.io/cloud-builders/gsutil
  args: ['-m', 'rsync' , '-r' , '-c', '-d', './public', 'gs://www.example.com']
```
This tells us a few things:
1. Use git (image located at `gcr.io/cloud-builders/git`) to clone my theme into `themes/tale`
2. Run `hugo` (image located at `gcr.io/{YOUR_PROJECT_ID}/git` (remember this from the last step?)) to build your static site
3. use `gsutil`'s `rsync` command to move our built static files from `public` to our Cloud Storage bucket

## Set up a build trigger
From cloud console:
- Go to cloud build -> build triggers
- Select "Add Trigger"
- Select your repo source
- Authenticate and select your repo
- Give your trigger a name (I used my domain name), select "branch" for trigger type, set "Branch (regex)" to "master", for "build configuration" select "cloudbuild.yaml", then click "create trigger"

What this is doing (behind the scenes), is putting a webhook into your repo (go check out your repo's settings and look at the webhooks).  This hook will be triggered whenever you push to master.

- Now you can create new posts:
`hugo new post/my-post-name.md`
- And push to master:
`git push`
And your site will automatically update itself!  You don't have to deal with admin pages, logins, security worries, or any of that.

## Where to go from here?
At this point your imagination can run wild.  You've got the basics of a hugo blog that updates by just pushing to master.  So - [learn](https://gohugo.io/documentation/) about Hugo and become an expert!
Other exercises include setting up a CDN in front of your GCS storage, setting up SSL, figuring out how to route a naked domain - there's plenty more to do.

Happy Blogging!
