---
title: How to write and publish articles on Hexo
tags: Hexo
date: 2016-08-05 22:02:46
---

# Writing Articles

1. Use `hexo new __ARTICLE_NAME__` to let Hexo automatically create a new `__ARTICLE_NAME__.md` file in `_posts` folder for you.
2. Edit the `__ARTICLE_NAME__.md` directly, and run `hexo server` to see the resulting webpage directly.
    * Use `<!-- more -->` in the post to control excerpt accurately

P.S. (Unrelated to the usage) [Equation vs Formula](https://www.mathsisfun.com/definitions/formula.html): A formula is a special type of equation that shows the relationship between different variables

# Writing Drafts

1. Use `hexo new draft __ARTICLE_NAME__` to let Hexo automatically create a new `__ARTICLE_NAME__.md` file in `_drafts` folder for you.
2. Edit the `__ARTICLE_NAME__.md` directly, and run `hexo server --draft` to see the resulting webpage directly.
3. Use `hexo publish draft __ARTICLE_NAME__` to publish the draft.
