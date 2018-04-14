---
layout: post
title:  "Notes - Creating Affiliate Links for Work"
date:   2015-03-16 13:43:48 -0800
categories: kajabi
---

Your SaaS project is taking off and you're making $$. Customers want to sell your product. Let's utilize Rails to create affiliate links so customer accounts will sell for you.

We'll want to generate an affiliate link akin to:

`https://<your_hostname>/r/<affiliate_token>`

This is made up of a protocol, your hostname and affiliate token - in that order. Rails url helpers will pick up current request protocol and hostname for us. This will make life easier. You'll see - let's get started.

Off your `Account` model, you'll want to generate a token. We'll assume you added an `affiliate_token` column as a string to your database. Might be safe to add an index for faster querying, too.

Manually generate affiliate tokens for existing accounts. For future customer accounts, we'll generate a token before they're created, like so:

#### app/models/account.rb
```
class Account < ActiveRecord::Base
  belongs_to :user
  
  before_create :generate_affiliate_token

  ...
  ...

  private
  
  def generate_affiliate_token
    self.affiliate_token = SecureRandom.urlsafe_base64(8)
  end
end
```

`SecureRandom#urlsafe_base64(length)` allows for hyphens and underscores which aren't what you normally see in tokens. It's beyond the scope of this tutorial, but stripping those characters to allow only A-Z, a-z and 0-9 could help more than hurt. Ruby's string `#gsub` and `#strip` are your friends.

To generate tokens for existing accounts, open a console and update their tokens:

#### Rails Console
```
> Account.find_by_id(<account_id>).
    update!(affiliate_token: SecureRandom.urlsafe_base64(8))
```

If you're keen on rake task migrations, it wouldn't be terrible to batch update accounts either..

#### lib/tasks/migrate.rake
```
namespace :migrate do
  desc "Update existing accounts with no affiliate tokens"
  task :update_affiliate_tokens_on_accounts => :environment do
    Account.where(affiliate_token: nil).find_each do |account|
      account.update!(affiliate_token: SecureRandom.urlsafe_base64(8))
    end
  end
end
```

Now we'll want a route with affiliate referral, `/r/`, between host and affiliate token.

#### config/routes.rb
```
Rails.application.routes.draw do
  ...
  ...

  get "/r/:token" => "affiliate_referral_redirects#show", :as => "referral_share_link"

end
```

The affiliate referral redirect controller should accept incoming requests with the token as a param. This controller can track affiliate accounts, clicks or whatever your affiliate strategy desires. Safe to assume you'll want to redirect the request to somwhere like a marketing page as well.

#### AffiliateReferralRedirectsController
```
class AffiliateReferralRedirectsController < ApplicationController
  def show
    affiliate = Account.find_by_affiliate_token(params[:token])

    if affiliate.present?
      # track affiliate conversion
      # track an affiliate click

      redirect_to affiliate_marketing_page_url
    end
  end
end
```

That's the gist! Throughout your app views, you can use `referral_share_link` to generate the full link _(protocol, host, affiliate token)_ for your users. Don't forget to pass the account affiliate token as a parameter!

#### Your affiliate view
```
<div class="referral-container">
  <input type="text" value="<%= referral_share_link_url(@account.affiliate_token) %>">
</div
```
