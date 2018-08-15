+++
title = "Create Cloud Storage"
date = 2018-08-14
draft = true
+++
## What will you learn today?
Today I'll walk you through creating a Google Cloud Storage bucket, and pointing to it from your own domain name.

By the end of this tutorial, you'll be able to serve index.html from your very own domain.

## Set up a domain
- Go to [Google Domains](https://domains.google) and register a new domain. (Here we'll use www.example.com)
- Set up a CNAME record for `www` pointing to `c.storage.googleapis.com.`
(image here)
- update `config.toml` in your repo to reflect your new domain name:
`baseURL = "http://www.thepedroblog.com"`

## Set up a Cloud Storage bucket
- Create a cloud storage bucket with the name of your domain and CNAME: www.example.com
(image here)
`gsutil mb gs://www.example.com`
- Note that you need to use a domain that you own and google has verified that you own, otherwise you'll get a 403 error.
- Make it public
- Add in `index.html` as the index file
