+++
draft = true
author = "Andy Huynh"
date = "2016-05-01T11:56:12-08:00"
linktitle = "RSpec be vs eq matchers"
title = "RSpec be vs eq matchers"
weight = 10
description = ""
+++

When writing tests and comparing strings, I would try both `be` and `eq` RSpec matchers. Whichever got green, I went with that. Don't assume, like I did.

Here's one way to remember it - let's say we're comparing an instance of a user's name to 'Jon'.

```ruby
scenario "User's name is Jon" do
  let(:user) { Fabricate(:user, name: "Jon") }
  expect(user.name).to be "Jon"
end

```

Running this test we'll get:

```
Failures:

  1) User's name is Jon
     Failure/Error: expect(user.name).to be "Jon"

       expected #<String:70298288643660> => "Jon"
            got #<String:70298288643700> => "Jon"
...
...
```

The `be` matcher expects both parties to actually BE the same entity. The identity of both `user.name` and the string `"Jon"` are expected to have the same object_id. We can test this in a console:


##### irb
```ruby
irb(main):001:0> str = "Jon"
=> "Jon" 
irb(main):002:0> "Jon"
=> "Jon" 
irb(main):003:0> str == "Jon"
=> true
irb(main):004:0> str.object_id
=> 70185646150880
irb(main):005:0> "Jon".object_id
=> 70185646097920
```

The equality matcher shows `str` is the same as the string "Jon". However, the identity of the two are different if we check their object ids.

The `eq` matcher is meant to check if two object's values are the same. The `be` matcher checks if the objects are ACTUALLY the same based on object id.

