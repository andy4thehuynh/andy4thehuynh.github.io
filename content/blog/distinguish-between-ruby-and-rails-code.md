+++
author = "Andy Huynh"
date = "2015-07-25T16:28:14-08:00"
title = "Distinguish Between Ruby and Rails Code"
+++

A common mistake among new developers is asking premature questions. Top shelf developers are best utilized as a last resort. Rails developers are expected to read and understand a TON of code. Hopefully you'll be reading good, idiomatic Ruby/Rails code. Jostling whether a class is Ruby or Rails is a common bump in the road.

There are clues to distinguish the difference. For example, when I come across `ActiveSupport::OrderedSOptions` in an application, it's blatant Rails code with ActiveSupport as a clear indicator. No questions necessary. However, if we come across something like:

```ruby
class CheckoutsController < ApplicationController
  skip_before_action :authenticate_site_user!

  def checkout
    ...
    ...
    token = CheckoutToken.encode(token_payload)
    ...
    ...
  end
end
```

A lesser experienced developer would assume CheckoutToken is something akin to a Rails class. I typically fire a rails console and instantiate the class. I can be sure it's Rails or a custom class if I get something in return. I follow up with an ack (recursive search) within a shell to see what the class does. If that doesn't work, 9/10 times it's a built in Ruby class or module I've never heard of.
