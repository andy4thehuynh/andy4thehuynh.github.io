+++
draft = false
date = "2016-09-13T20:45:21-07:00"
title = "That One Time to Mutate Ruby Objects"

+++

Everytime an object or variable is invoked by a non mutating action, the object remains untouched. This is particularly relevant when passing objects around via method calls. If the passed hash gets mutated along the chain, receiving class method require more responsibility to handle ambiguous data. New hash keys could be introduced or removed which changes your expectation of what you're getting. This is cautionary coding we want to avoid.

Seldom is the case to mutate objects holding state or data. However, one reason is if mutation convention already exists. It's most likely safe to add mutating methods to it.


```
class ApplicationHelper

  def body_classes
    classes = []
    
    classes << "admin" if admin?
    classes << "#{params[:controller]}"
    classes << "environment" if Rails.env.test?
    
    classes.concat(@extra_body_classes) if @extra_body_classes
    classes.join(" ").tr("/_", "-")
  end
end
```

In the above example, it would not make sense to use a non mutating operand to our `classes` local variable. Using mutated methods like `Array#concat` or another shovel operand are fine since `classes` is being mutated throughout the method. Being mindful of object mutation could save headaches when dealing with passed objects.
