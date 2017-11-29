---
layout: post
title:  "Renew SSL Certificates using DNSimple"
date:   2017-11-28 13:43:48 -0800
categories: devops
---

SSL certificates expire on a yearly basis. We renew it to stay protected from fraudulent behavior along the wire. Without it, we're prone to malicious hackers stealing sensitive information (credit card info, passwords). Here's how we renew SSL certs with DNSimple - yes, there's pictures.

- When your certs are close to expiring, you should notice an expiration alert in your DNSimple dashboard. Click renew certificate to start the process.

![dash_alert](/assets/posts/renew_ssl_certs/1_dash_alert.png)

- If there's no hiccups, you'll get this confirmation in your browser. DNSimple takes hours to a couple days to process a renewal, so hang tight. Important to take care of renewals days prior to its expiration to avoid.

![order_confirmation](/assets/posts/renew_ssl_certs/2_order_confirmation.png)

- The **action required** email is the one we're waiting for.. see below.

![action_rquired_email](/assets/posts/renew_ssl_certs/3_action_required_email.png)

- Head back into your dashboard click the domains header tab. Select your domain and check out the SSL Certificates tab. Select the domain link with the latest renewal date.

![renew_cert](/assets/posts/renew_ssl_certs/4_renew_cert.png)

- Scroll till your find **Install the SSL Certificate**. It's a scary link title, but it just takes you to an instructions page with your infrastructure setup. I followed the Heroku instructions with no qualms. Before you do, you can verify everything actually worked by finishing this tutorial.

![install_cert](/assets/posts/renew_ssl_certs/5_install_cert.png)

- To verify the state of your certificate: head into a Chrome browser (YYMV with another browser), open your developer console and find your security tab. Click **view certificate**.

![view_cert](/assets/posts/renew_ssl_certs/7_view_cert.png)

- You should see the expiration date reflected. From here, follow the instructions on DNSimple to update your certificates. Come back here and you should see it change.

![confirm_cert](/assets/posts/renew_ssl_certs/6_confirm_cert.png)

The last part is the most important to me because it validates that the certs successfully updates. It almost feels like doing TDD seeing red failing tests go green. Anyways, that's it!
