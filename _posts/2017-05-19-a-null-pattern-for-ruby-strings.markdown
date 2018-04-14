---
layout: post
title:  "A Null Pattern for Ruby Strings"
date:   2017-05-19 13:43:48 -0800
categories: ruby
---

Super simple: you check the presence of an object to do work on it. We see this in code all the time.

### Common presence check pattern
```
if params[:coupon_code].present?
  params[:coupon_code].upcase
else
  params[:coupon_code]           # Let's say this would return nil
end
```

This dance isn't special and we can avoid it with a Ruby trick by typecasting the string.

### irb
```
> nil.to_s
=> ""
> "".upcase
=> ""
```

Since typecasting nil to a string will always return a blank string, we can call string methods safely. We can sum up the first example as:

### Short and sweet by coercion
```
params[:coupon_code].to_s.upcase
```

Whether there's content in the string to upcase or a blank string, you'll get a result and won't have to worry getting a `NoMethodError` on nil. Much cleaner and succinct, don't you think?
