+++
author = "Andy Huynh"
categories = ["Git"]
date = "2016-08-30T11:56:12-08:00"
title = "Your SHA is Your Friend"
description = "Save your Latest SHA when Rebasing to Master"
type = "post"
+++

You'll want to do yourself a favor before making major rebases from master to your feature branch.

#### Save your latest SHA

Jot down the sha of your latest commit. This has saved my ass several times. You'll avoid sifting through the dreaded reflog trying to figure out where you fucked up. Execute a `git log` and copy the SHA to your notes. Use `git reset --hard <your_sha>` to go back to that state.

#### Delete your local branch and pull from Github

If you forgot to jot down your SHA, hopefully you pushed up your latest changes to Github. In this case, try checking out to master and delete your branch: `git br -D <your_branch>`. Fetch your branch again: `git fetch`. This should pull a fresh version of your branch with your latest commit before the rebase.
