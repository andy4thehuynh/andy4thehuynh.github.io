+++
author = "Andy Huynh"
categories = ["Rails"]
date = "2016-05-18T16:28:14-08:00"
title = "Understand Javascript Data Tags With Me"
description = "Data Attributes in Rails helpers"
type = "post"
+++

I was given the task of creating a copy to clipboard button for readonly inputs. This feature would be omnipresent in the codebase, a great candidate for a helper. The trickiness came when implementing attributes without values in Rails helpers. 

As an aside, copying to a clipboard is widely known to be trouble to work with across different browsers and mobile devices due to Javascript and security issues with a user's clipboard.

Without worrying about the copy button, we want our input to look like this:

##### HTML
```
<div class="form-group">
  <div class="input-group">
    <input type="text" readonly data-autoselect value="http://purchase-me.com/checkout">
  </div>
</div>
```

Notice the `readonly` and `data-autoselect` attributes with NO values. After experimenting with null and blank strings in Rails helpers, this worked fine:

##### app/helpers/clipboard_helper.rb
```ruby
module ClipboardHelper
  def clipboard_button(id:, input_value:)
    content_tag :div, class: "input-group" do
      concat tag(:input, type: "text", readonly: '', data: { autoselect: '' }, value: input_value)
    end
  end
end 
```

It doesn't look EXACTLY like what we want but it renders properly in the browser.

##### HTML
```
<div class="form-group">
  <div class="input-group">
    <input type="text" readonly="readonly" data-autoselect="" value="http://purchase-me.com/checkout">
  </div>
</div>
```

Apparenty setting blank strings to readonly or javascript data attributes in Rails helpers plays nice with browsers. Who knew!
