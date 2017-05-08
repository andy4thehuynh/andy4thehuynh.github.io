+++
author = "Andy Huynh"
categories = ["Git"]
date = "2016-09-02T16:28:14-08:00"
title = "Use Git To Debug Your Problems"
description = "Git Stash and Diff as Experimental Tools"
type = "post"
+++

A common scenario working on a new feature is to recklessly code and _pray_ it all works. Often times, this **MIGHT** work. Sometimes, shit hit the fan.

Here's a Vim/Tmux/Git trick to view changes and incrementally add code to debug your problems.. all on a **single screen**.

#### 1. Open three panes in Tmux: Vim and two terminal shells.
You'll want a pane to edit code, another to see diffs and the other to run a stash.

#### 2. Get clean with Git.
Make sure you have a clean spot to revert before experimenting.

#### 3. Experiment!
From your clean slate, start experimenting. Experiment until you're stuck.

#### 4. Stash, diff and incrementally see what works.
Here's the secret sauce: save your changes in Vim and move to an open terminal. Execute `git diff`. These are the changes you just made from your clean slate. **KEEP THIS OPEN!**

Now, move to an available shell and `git stash` your changes. We're now at a clean slate but can still see the code we were experimenting with in one of our terminal panes.

Any open vim files with stashed changes will have to be resetted with `:e!`.

From here, your staging area is clean and experimental code is in the `git diff` shell. I typically start from the easiest lines and see what works and what doesn't... adding as I go.
