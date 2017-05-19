+++
author = "Andy Huynh"
categories = ["Ruby"]
date = "2015-06-24T14:31:46-07:00"
title = "Fixing a version mismatch of Spring, Locally"
description = ""
type = "post"
+++

You may have added Spring to a differenet project as a Ruby gem. Spring reads it and passes this error. The band aid solution is to `bundle update spring` which will temporarily ask you to add your Gemfile.lock.

This is cool if you're coding solo and don't care your version. Collaborating on the project might get more tricky.

<img src="/images/spring-version-mismatch-error.png" alt="spring version mismatch" />

```
> cd /Users/andyhuynh/.gem/ruby/2.2.0
> ls | grep -i spring
```

Delete all versions of Spring not named the one you're working with in your project.
