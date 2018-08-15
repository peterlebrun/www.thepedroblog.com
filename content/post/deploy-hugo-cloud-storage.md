+++
title = "Deploy Hugo Cloud Storage"
date = 2018-08-15
draft = true
+++

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
  args: ['-m', 'rsync' , '-r' , '-c', '-d', './public', 'gs://www.example.com']
```
- This tells us a few things:
1. Use git (image located at `gcr.io/cloud-builders/git`) to clone my theme into `themes/tale`
2. Run `hugo` (image located at `gcr.io/{YOUR_PROJECT_ID}/git` (remember this from the last step?)) to build your static site
3. use `gsutil`'s `rsync` command to move our built static files from `public` to our Cloud Storage bucket

## Step 6: Set up a build trigger
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
