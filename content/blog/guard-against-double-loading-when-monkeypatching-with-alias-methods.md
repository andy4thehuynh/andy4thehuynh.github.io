+++
draft = false
author = "Andy Huynh"
date = "2016-09-09T17:26:11-07:00"
title = "Guard against Double Loading when Monkeypatching with Alias Methods"
+++

Before we discuss how to guard against a double load when monkeypatching with an alias method, know that a `stack-level-too-deep` error is a good indicator your method has already been defined. It loaded already and is trying to load again.

This is commonplace in backgroudn jobs where a class might get loaded more than once. It's unfortunate for your monkeypatched method being called because you'll go through an infinite loop which leads to the error above.

For this example, we'll use Ruby's Money gem. In particular, [Money#format](https://github.com/RubyMoney/money/blob/39b617cca8f02c885cc8246e0aab3e9dc5f90e15/lib/money/currency/heuristics.rb#L92).

```
class Money
  alias_method :original_format, :format

  def format(*rules)
    ...
    ...
    ... your format implementation
    ...
    ...

    original_format(rules)
  end
end
```

The important lines are `alias_method :original_format, :format` and calling `original_format(rules)`. We alias Money's format method so we can define our own before calling the original format method from Money. However, if this somehow gets loaded twice, the interpreter will get stuck an infinite loop calling the already defined original_format. Thus a stack-level-too-deep error is raised.

To avoid this, we check if the method has been defined. If it has, we'll use that cached version like so:


```
unless Money.method_defined?(:original_format)
  class Money
    alias_method :original_format, :format

    def format(*rules)
      ...
      ...
      ... your implementation before Money calls its #format method.
      ...
      ...

      original_format(rules)
    end
  end
end
```

Now, if original_format has been defined, we'll use the cached version and avoid unwarranted errors.
