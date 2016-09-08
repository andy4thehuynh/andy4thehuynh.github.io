+++
draft = true
date = "2016-09-06T20:45:21-07:00"
title = "Prefer Non Mutating Actions in Ruby"

+++

Go for non-mutating actions preferably. Hashes for example when it gets passed along to another element and you call the original class with the hash, it has keys it doesn't expect or is missing stuff because you mutated it somewhere else.

You problem was plus vs concat in the application helper class for add_admin_body_classes
