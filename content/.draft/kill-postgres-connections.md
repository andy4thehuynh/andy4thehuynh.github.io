+++
draft = true
author = "Andy Huynh"
date = "2016-08-27T17:26:11-07:00"
title = "Kill Postgres Connections"
+++

Getting the warning:

PG::ObjectInUse: ERROR:  database "kajabi-portals_development" is being accessed by other users
DETAIL:  There are 4 other sessions using the database.
: DROP DATABASE IF EXISTS "kajabi-portals_development"

You can't `rake db:drop` because you have active connections to your Postgres database. Postgres doesn't let you disconnect when you have active connections. This could be from using Tmux or a window you don't know you have up.

#### 1. Kill your servers, consoles, sidekiq jobs and stop spring.
Kill your servers, 

#### 2. PG KILLCON
Pg_killcon should work.

#### 3. Rip Ruby processes from Activity Monitor.
Fire up your Activity Monitor. Search for Ruby. Kill those processes individually (you can't catch delete them). You should be good.
