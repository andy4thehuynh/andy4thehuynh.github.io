+++
author = "Andy Huynh"
date = "2016-04-01T17:26:11-07:00"
linktitle = "Dirty Rails Schema Fixes"
title = "Dirty Rails Schema Fixes"
weight = 10
description = "Switching git branches with outstanding migrations leaves your schema in awkward states. Try these quick fixes."
+++

You might get asked to switch tasks while working on a feature. Outstanding migrations are still in your schema and other conflicting migrations might put your schema in an inaccurate state. Here are some quick fixes:

## Rollback your schema
In your branch with an outstanding migration, find the timestamp and run `bundle exec rails db:migrate:down VERSION=<timestamp>`. Checkout db/schema.rb and switch branches to continue working.

## Start with a fresh schema
Akin to week old, stale curry in tupperware, sometimes it’s best to ditch the whole thing and start fresh. We can drop our database, create a new instance then delete our current schema. Running `rake db:migrate` will create a new schema that reflects migration files in `db/migrate`. Run your seed and it's like brand new. This is especially useful if you don’t care about the data you accrued in your dev database.

``` bash
 >git:(master) bundle exec rake db:drop
 >git:(master) bundle exec rake db:create
 >git:(master) git rm delete db/schema.rb
 >git:(master) bundle exec rake db:migrate
 >git:(master) bundle exec rake db:seed
```
