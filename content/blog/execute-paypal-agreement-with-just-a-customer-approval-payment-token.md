+++
author = "Andy Huynh"
categories = ["PayPal", "Hack"]
date = "2017-05-09T08:11:27-07:00"
title = "Execute PayPal Agreement with JUST a Customer Approval Payment Token"
description = "Use the Payment token from customer approval to execute an Agreement"
type = "post"
+++

We come across the problem of successfully creating and activating a [PayPal billing plan](https://developer.paypal.com/docs/integration/direct/billing-plans-and-agreements/#create-a-plan) and not being able to create and [execute a billing agreement](https://developer.paypal.com/docs/integration/direct/billing-plans-and-agreements/#execute-an-agreement) attached to the plan. PayPal sandbox says a billing plan is created, and we create a billing agreement successfully. There's an interesting dance where PayPal asks you to get customer approval before you can marry the plan and agreement with the execute command. This part sucks.

It sucks because when you get the customer approval and get your payment token back, you are stuck in a position where PayPal only gives you limited data. Even worse, your billing agreement that you created doesn't have an ID yet. You can't simply find your agreement and execute it with the Ruby SDK like so: 

```
> agreement = PayPal::SDK::REST::Agreement.find(<agreement_id>)
> agreement.execute
```

You can't even find it by the payment token you get back from creating the agreement. Being a Rubyist, I want to have a create and execute action in my controller where I create the agreement and then hit the execute action to finish it off. However, execute needs to have the agreement in order to be executed.

Having dug throught the source code, shout out to Codegangsta at Kajabi, we found the true nature of PayPal's REST API data types to act like a hash. Each data type only requires certain things per action. In our case, Agreement#execute only requires the token. A new instance of this will execute it, thus finishing the dance.

Coming from Rails land, this feels weird because we create an instance and expect to reuse that instance in order to finish creating it.. must like how we create a new instance of an ActiveRecord object like:

```
> user = User.new
# add some stuff to the user
> user.create
```

Creating a new instance of an agreement then executing with the paymentToken returned from the customer approval gives the Agreement data type all the info it needs (to which the data most likey lives deep in the token) to execute. 

```
> agreement = PayPal::SDK::REST::Agreement.new(token: <paymentToken>)
> agreement.execute
```
