+++
draft = true
author = "Andy Huynh"
date = "2016-08-27T17:26:11-07:00"
title = ""
+++

Want to allow blank but not nil.

Select drop down.  You want your options array to take hashes but not allow them to be nil. Must write a custom validator.

if options.nil?
  errors.add(:base, "cannot be nil")
end

Test it:


If you instantiate an object with ActiveRecord#new.. don't expect validations to kick in until you call #valid? on the object.
