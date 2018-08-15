+++
title = "Create Hugo Blog"
date = 2018-08-13
draft = false
+++
## What will you learn today?
Today I'll walk you through setting up a blog with Hugo.  This tutorial is aimed at someone with technical skills, comfortable with markdown.

By the end of this tutorial, you'll have a working Hugo blog that you can run and build locally.

## Getting started in five lines
```
brew install hugo              # Install Hugo
hugo new site hello-world      # Create a site
cd hello-world
hugo new post/first.md         # Create your first post!
echo "## Hi!" >> post/first.md # Write some markdown
hugo server -D                 # Start dev server
```

## Installing a theme
Choose your theme from [Hugo's Theme site](https://themes.gohugo.io/).  I'm currently using [Tale](https://themes.gohugo.io/tale-hugo/).

```
git clone https://github.com/EmielH/tale-hugo.git themes/tale
```

Don't version control your themes, it's really not needed.
```
echo "themes/*" >> .gitignore # Don't version control this
```

## Building for production
```
hugo
```
Seriously.  That's it.

Also, make sure you don't version control your built site:
```
echo "public/*" >> .gitignore
```

## Final Steps
Set up your git repo and push
```
git init
git remote add origin {YOUR_REPO_URL}
git push -u origin master
```

Come back tomorrow when we'll set up a storage bucket and route your domain to it.