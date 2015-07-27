+++
description = ""
date = "2015-06-24T14:31:46-07:00"
title = "Ruby Enumerable Nil Guard"

+++

Ruby is notorious for its enumerable library. I frequent Ruby's `Array#map` to iterate and modify a collection of data. A common occurrence is looping over a collection and getting an `undefined method _ for nil:NilClass`.

``` bash
> irb
>
> arr = [1, 2, 3]
> arr.map { |x| x * 5 }
=> [5, 10, 15]
> arr << nil
> arr << 5
=> [1, 2, 3, nil, 5]
> arr.map { |x| x * 5 }
NoMethodError: undefined method '*' for nil:NilClass
    from (irb):in `block in irb_binding`
    from (irb):in `map'
```

Map has trouble looping over nil values. This is where Enumerable#compact is handy. It will rip nil values from our collection which allows map to function properly. Here's one solution to defend against such cases.

``` bash
> arr = [1, 2, 3, nil, 5]
> arr.compact.map { |x| x * 5 }
=> [5, 10, 15, 25]
```

[Enumerables](http://ruby-doc.org/core-2.2.2/Enumerable.html) are powerful on their own, but when combined with other enumerables can yield total control over your data. I would get acclimated with enumerables since collections are considered rudimentary for any professional developer.
