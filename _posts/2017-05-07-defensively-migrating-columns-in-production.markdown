---
layout: post
title:  "Defensively Migrating Columns in Production"
date:   2017-05-07 13:43:48 -0800
categories: rails
---

Let's say we have an `Account` class with an address column. We got word a home address AND a work address is in play. Apparently addresses in our app are complex creatures and we have a `has_one`/`belongs_to` relationship. We'll have to rename and add a column. Renaming the column is the point of this post and it will get tricky, especially with production data.

When migrating data, we have a short window where Heroku will take roughly 60 seconds to migrate the data. Customers will 500 in this short window when hitting the address. This little defensive move will help prevent that and give more granular control.

So if we rename `address_id` off `Account` to be `home_address_id`, cool. We change all places in our app where this is called and can implement something like this:

```
class Account
  has_one :home_address

  unless ENV['ADDRESS_FALLBACK'] == 'false'
    def home_address
      if respond_to?(:home_address_id)
        super
      elsif respond_to?(:address_id)
        HomeAddress.find(:address_id)
      else
        raise "Can't find Home Address for account_id: #{id}"
      end
    end
  end
end
```

This is awesome for many reasons. We forget to set a config in Heroku called `ADDRESS_FALLBACK` before we push? No big deal, this code works out the box when we push because our environment variable will already be nil. Our code is working. We'll set the flag to false after the migration has ran so we don't make expensive find queries. This will also keep our tests passing until we rip this method. The ENV check buys us time in case we can't re-deploy right away.

The raise will swallow 500s and send errors to services like Honeybadger if you care to investigate.

We're simply migrating an address, but this is especially important when renaming an important column like a payment integration that touches various parts of our app.

Again, this is a temporary solution to prevent 500s in production as we migrate `address_id` to `home_address_id`.
