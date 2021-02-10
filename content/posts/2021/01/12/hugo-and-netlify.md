---
title: "Hugo and Netlify"
date: 2021-01-12T15:07:20-05:00
draft: false
description: "My brief overview of launching my personal blog."
tags: ["hugo","netlify","blog"]
---

Over the last couple of years I have played around with Jekyll, Gatsby, Hugo, among
a few others.  Hugo, in my experience, required the least amount of work out of me
to accomplish my goal.  Personally, anything that values the end user's time invest 
has high marks in my book.  I think any software, service, or product's end goal
should be to make whatever task easy.  For me, Hugo seemed to cover that.

That said, I also wanted to become familiar with a [JAMstack](https://jamstack.org/) implementation.
From what I have read, its practically minutes from anyone's ability to launch a Hugo site
on Netlify.  So with some vigor under my belt I set off to accomplish this seemingly simple launch.

I won't banter on much about the steps here.  I followed the simple steps from [Hugo's quickstart](https://gohugo.io/getting-started/quick-start/).  For the moment, I thumbed through the catalogue of available themes
to find something suitable to my current needs.  I made some minor tweaks and added my own little spin on a 
few elements.  Such as setting the emoji processing in the titles using the [`emojify`](https://gohugo.io/functions/emojify/) processor.

Now, the real meat and potatoes of it all.  How can a continuous deployment of the website happen to Netlify?
Well, it just so happens that all you need is a `netlify.toml` configuration file in the root of
your repository and Netlify will make the magic happen.

**Example `netlify.toml`:**
```toml
[build]
publish = "public"
command = "hugo --gc --minify"

[context.production.environment]
HUGO_VERSION = "0.80.0"
HUGO_ENV = "production"
HUGO_ENABLEGITINFO = "true"

[context.split1]
command = "hugo --gc --minify --enableGitInfo"

[context.split1.environment]
HUGO_VERSION = "0.80.0"
HUGO_ENV = "production"

[context.deploy-preview]
command = "hugo --gc --minify --buildFuture -b $DEPLOY_PRIME_URL"

[context.deploy-preview.environment]
HUGO_VERSION = "0.80.0"

[context.branch-deploy]
command = "hugo --gc --minify -b $DEPLOY_PRIME_URL"

[context.branch-deploy.environment]
HUGO_VERSION = "0.80.0"

[context.next.environment]
HUGO_ENABLEGITINFO = "true"
```