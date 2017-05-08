+++
author = "Andy Huynh"
categories = ["Code Review"]
date = "2016-10-22T17:26:11-07:00"
title = "Depend on Not Depending on Dependencies"
description = "A simple code review best practice."
type = "post"
+++

When reviewing a pull request that contains a new gem, search it in RubyGems and check the homepage. Most homepages lead you to Github. Here, we can view its dependencies.

This check will force us to decide whether this gem, clothed in sheep skin as a mere dependency, might bite us in the butt because it depends on other libraries to function. Dependencies live in the gemspec file.

Coupling your app to libaries with many dependencies should stop you dead in your tracks to second guess your decisions. Sometimes dependency reliant libraries are worth it if they affect a huge chunk of your app and are stable. Just my two cents.
