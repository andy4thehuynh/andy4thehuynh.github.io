+++
author = "Andy Huynh"
categories = ["Ruby"]
date = "2016-03-24T17:26:11-07:00"
title = "Wrap Your Head Around Memoization"
description = "ˌmemərəˈzāSHən | noun"
type = "post"
+++

Ruby has a memoization operator, `||=`. In a nutshell, we use it for caching the result of a method to avoid querying it over and over. Our application can function faster as a result! There's a particular use case to keep in mind.

Let's say you're finding an account and want their credit card. It's common to query and check if your resource (account), is present first. We're being prudent so we don't encounter a `NoMethodError` on NilClass, a nil account.

``` ruby
class Account
  def find_credit_card
    if purchasing_account.present?
      purchasing_account.credit_card
    end
  end

  private

  def purchasing_account
    Account.find(id)
  end
end
```

This totally works, but is inefficient and slow. In `Account#find_credit_card`, we're asking `purchasing_account` if it is present, and if it is to find the credit card. Ruby will query `purchasing_account` twice which will slow down our app in the long run. 

``` ruby
class Account

  # ... 

  private

  def purchasing_account
    @purchasing_account ||= Account.find(id)
  end
end
```
Now when we ask for the purchasing account's credit card, we will use the cached value from the presence check.
