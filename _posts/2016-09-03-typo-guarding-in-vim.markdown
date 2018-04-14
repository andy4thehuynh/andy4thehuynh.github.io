---
layout: post
title:  "Typo Guarding in Vim"
date:   2015-03-16 13:43:48 -0800
categories: vim
---

I like to code recklessly. Spelling errors aren't uncommon when this happens. Defining variables and calling them in different places where I expect them to work and they DON'T work is a problem. There's a command with Vim that's handy for this situation.

Hover your Vim cursor over the variable you **instantiate** and `SHIFT *` over it. This will highlight each variable with the same variable name, one by one.

<img src="/assets/posts/images/spelling_trick_with_vim.png" alt="spelling_trick_with_vim" />

It allows you to visually see each variable and if it's defined with accurate spelling or if you fucked up!
