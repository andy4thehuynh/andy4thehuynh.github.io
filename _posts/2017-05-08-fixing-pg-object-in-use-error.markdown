---
layout: post
title:  "Fixing PG::ObjectInUse: ERROR"
date:   2017-05-08 13:43:48 -0800
categories: postgres
---

Here's why and how you fix this error:

```
PG::ObjectInUse: ERROR:  database "<MY_DATABASE>" is being accessed by other users
DETAIL:  There are 4 other sessions using the database.
: DROP DATABASE IF EXISTS "<MY_DATABASE>"
```

You can have a console open in a shell, your server running, another console open and maybe try opening another Rails app in a Tmux session. You're spinning multiple Ruby processes and Postgres isn't down with it.

You can't perform normal tasks like `rake db:drop` or migrate because you have active connections to your Postgres database. Postgres doesn't let you disconnect when you have active connections.

Here are options to fix that:

#### 1. Kill your servers, consoles, sidekiq jobs and stop spring.
`CTRL C` everything. That'll halt execution of servers, consoles and jobs. Also, `spring stop` has magic that cures random things like this.

#### 2. PG KILLCON
[Rolf Blindheim](https://github.com/rhblind) wrote a script that'll kill your PG connections. Download his [script](https://gist.github.com/rhblind/4492988) and run it as a shell command.

#### 3. Rip Ruby processes from Activity Monitor.
Fire up your Activity Monitor. Search for Ruby. Kill those processes individually (you can't catch delete them). That'll do it!
