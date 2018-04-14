---
layout: post
title:  "RSpec eq and be Matchers"
date:   2016-08-29 13:43:48 -0800
categories: rspec
---

I confuse RSpec's `be` and `eq` matchers when writing specs. It's basically one or the other and the specs pass. Better you understand them to be a better programmer. Here's my train of thought:

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

The `be` matcher expects both parties to actually **BE** the same entity. Both `user.name` and the string `"Jon"` are expected to have the same **object_id**. We can test this in a console:


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

The equality matcher, `eq`, is what you expect the string value to **LOOK** like. Going back to our earlier example:

```ruby
scenario "User's name is Jon" do
  let(:user) { Fabricate(:user, name: "Jon") }
  expect(user.name).to eq "Jon"
end

```

You'll get:

```
Finished in 1.12 seconds (files took 13.85 seconds to load)
1 example, 0 failures

Randomized with seed 4227
...
...
```

The `eq` matcher is meant to check if two object's values are the same. The `be` matcher checks if the objects are **ACTUALLY** the same based on object id.
