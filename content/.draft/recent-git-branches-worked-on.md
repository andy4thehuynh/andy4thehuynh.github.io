+++
draft = true
author = "Andy Huynh"
date = "2015-06-25T17:26:11-07:00"
title = "Recent Git Branches Worked On"
+++

`git br --sort=-committerdate`

Unix's head reads the first few lines of an input file and outputs them to standard output.

```
git br --sort=-committerdate | head
```


As an aside, you may have LOTS of branches. Branches are just files and are cheap. However, you see 173 branches and you'll want to purge em too.

Count branches
```
git br | wc -l
```
