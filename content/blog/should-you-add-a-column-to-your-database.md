+++
author = "Andy Huynh"
categories = ["Git", "Postgres"]
date = "2017-05-10T16:28:14-08:00"
title = "Should You Add a Column to your Database"
description = "Think twice, there's a reason to"
type = "post"
+++

There's columns you'll want to query from your database. However, third party integrations (like Stripe or PayPal), in particular, might urge you NOT to add a column to your database, depending on your setup.

Let's state the obvious: a basic `User` class will most likely have an email column. You'll be querying this often as a SaaS for customers. A database column makes complete sense is common sense.

A good rule of thumb to add a database column when working with third parties is if you'll be querying them often. Let's say we have a model called StripePaymentTransaction that serializes our response from Stripe as JSON in Postgres. Serializing integration responses to your database seem like a natural flow that is used in the industry. This object serves as an example of info we can directly pull from the object and when to add a column to our model.

Reaching through our StripePaymentTransaction's Stripe object for trivial information like if we're in livemode would do a diservice to our integrity of our database. However, the Stripe object's amount could be useful to our customers and would probably show up throughout the app. This is the meat and potatoes of this post. Add columns you'll use alot, leave the others you can hide deep in third party objects.

I used to think every property off an ActiveRecord model should belong in the database. Don't be fooled.  Having to reach deep down an integration object would make Demeter roll over in his grave (google search for Law of Demeter), but if you'll use it often - make it a column.
