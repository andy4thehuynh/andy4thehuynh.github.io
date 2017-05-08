
draft = true
author = "Andy Huynh"
date = "2016-08-27T17:26:11-07:00"
title = ""
+++

You had random commits that didn't match with master. However, you had commits you wanted to save and open on a different branch. Squash commits into one commit on feature branch. Check out a new branch on master and cherry pick for that squashed commit.

==> git rebase -i HEAD~<number_of_commits>     // squash
==> git checkout master
==> git checkout -b <new_branch>
==> git cherry-pick <squashed_commit_sha>
