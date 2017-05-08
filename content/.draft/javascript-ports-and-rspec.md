+++
draft = true
author = "Andy Huynh"
date = "2016-08-27T17:26:11-07:00"
title = ""
+++

If you're testing and verifying routes with the js driver, you'll want to explicitly set the port in your expectations.

Capybara.server_port = 4242
referral_share_link_url(affiliate_token, host: AppConfig.host, port: Capybara.server_port)

Only when you have js:true meta tag.
