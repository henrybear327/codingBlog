---
title: Hexo Installation Notes
date: 2016-08-02 23:28:08
categories: Hexo
tags: Hexo
---

# My Hexo setting notes

Just random stuff, may be outdated.

<!-- more -->

# Prerequisites

## Ubuntu

* We will need to have NPM and nodejs installed
  * `curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -`
  * `sudo apt-get install -y nodejs`
  * `sudo apt install npm`
* Install Hexo core components
  * `npm install hexo-cli -g`
  * `npm install hexo-deployer-git --save`, so Hexo site deployment using git will work properly

## Mac

* Use `homebrew` to install `npm` and `nodejs`
* Install Hexo core components
  * `npm install hexo-cli -g`
  * `npm install hexo-deployer-git --save`, so Hexo site deployment using git will work properly

# Hexo Usage

* Create a new project:
  * run `hexo init __FOLDERNAME__` in the directory that you would like to save the Hexo project
  * `git init` the project folder. __NOTICE__: All files to be displayed must be on [`gh-pages`](https://pages.github.com/
    * Setup `git remote` and `git branch` according to the official documentation!
      * `.deploy_git` is the place where the automatic git commits go
) (set this to default branch to avoid further problems)
  * Setup `_config.yml` properly (Notes are down below)
* Create a new article: `hexo new "__ARTICLE_NAME__"`
* The __most useful__ deployment command is `hexo generate --deploy`
* Some basic commands:
```
hexo clean
hexo generate
hexo deploy
hexo server
```

# Notes for `_config.yml` (top level one)

There are only a few parts to change when setting up a whole new project. The following examples are all taken from my currently working Hexo site.

## Site

```
# Site
title: Coding Notes
subtitle:
description:
author: Henry Tseng
language: en
timezone: Asia/Taipei
```

## Project root

```
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: https://henrybear327.github.io/
root: /codingBlog/
permalink: :year/:month/:day/:title/
permalink_defaults:
```

## [Themes](https://bemg.github.io/blog/2016/08/05/Hexo-install/#主題)

To replicate the theme that I am using, there are only three steps to do.

First, clone the *next* theme into `themes` folder by running `git clone https://github.com/iissnan/hexo-theme-next themes/next`.

Second, change the theme name in `_config.yml` at **project root**. Notice that each theme has their own `_config.yml`.
```
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next
```

Third, go to the `_config.yml` in `themes/next/`, and look for `Schemes`. I am currently using the `Mist` scheme.
```
# Schemes
#scheme: Muse
#scheme: Mist
scheme: Pisces
```

P.S. Special thanks to BeMg for helping me out on changing themes.

## Deployment settings

```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: git@github.com:henrybear327/codingBlog.git
  branch: gh-pages
  message: 'Updated at:{{now("YYYY-MM-DD HH:mm:ss")}}'
  name: '...'
  email: '...@......'
```

# [Create special pages for `next` theme](https://github.com/iissnan/hexo-theme-next/wiki/创建标签云页面)

1. Run `hexo new page "tags"`
2. Go to `tags/index.md` and copy-paste the following content
```
title: All tags
date: 2016-08-09 01:47:00
type: "tags"
---
```
3. Run `hexo new page categories`
4. Go to
```
title: All Categories
date: 2017-05-01 09:45:00
type: "categories"
comments: false
---
```

# Google Analytics

Simply update the `google_analytics` field in the theme's `_config.yml`

# Code highlight

`highlight_theme: night eighties`

# Other settings

Check [this](http://theme-next.iissnan.com/getting-started.html) out!
