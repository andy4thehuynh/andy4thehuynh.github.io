---
layout: post
title:  "My Production Deployment Checklist"
date:   2016-10-09 13:43:48 -0800
categories: deploy
---

This post serves as a deploy catalog.

### Push to Github:
1. Pull the latest changes from master.
2. Switch to production branch. Update that shit.
3. Check logs and diffs for both branches. Sanity check.
  - `git diff production..master`
  - Take note of features you're merging.
4. `git merge master`
5. `git push origin production`

### Push to staging:
1. Check heroku for greens: `heroku status`
2. `git push staging production:master && heroku run rake db:migrate -r staging && heroku restart -r staging`
  - Push your production branch to staging's master branch. Migrate schema changes, if needed and restart the app.
3. Login to staging and poke around the features you just merged.
4. Check JS console for any weird errors.

### Push to production:
1. `git push production production:master && heroku run rake db:migrate -r production && heroku restart -r production`
2. Make sure your latest Heroku SHA release is the same as your production branch.
  - `heroku releases -r production` 
3. Creep on HoneyBadger for any extraordinary bombs. 
  - "Unknown column error" is a normal error.

Unix trick: Use `fc` to edit and execute your latest shell command.
