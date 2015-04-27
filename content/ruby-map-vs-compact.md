+++
description = ""
date = "2015-04-26T14:31:46-07:00"
menu = "main"
title = "Ruby Map vs Compact"

+++

I use Ruby's `Array#map` to iterate and modify a collection of data. Let's explore a simple scenario where we need more than #map.

```
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

As you can tell, #map can't operate over nil values. This is expected and a common case when dealing with Ruby #map. Luckily, we have #compact which will omit nil values from our collection. This way, we can expect #map to act accordingly and not blow up on us. 

```
> arr = [1, 2, 3, nil, 5]
> arr.compact.map { |x| x * 5 }
=> [5, 10, 15, 25]
```

Ruby's #compact dipped our nil values and we are left non-nil values #map can comprehend. [Enumerables](http://ruby-doc.org/core-2.2.2/Enumerable.html) are powerful on their own, but when combined with other enumerables can yield total control over your data. Learn them and use wisely!
