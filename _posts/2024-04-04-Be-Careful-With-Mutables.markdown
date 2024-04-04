---
layout: post
title:  "Python: Be careful using mutables in python classes"
date:   2024-04-04 17:18:00 +0200
categories: blog
---

# Python: Be careful using mutables in python classes
Object oriented programing is a very effecient and intuitive way of operating on data structures. The basic idea is to create objects that have their own data structures, functions and logic. I am currently developing my own parser in Python and started to design classes that would be instansiated as objects. What I discovered was that, when working with mutable data types such as lists, you might not get the result you expected.

## The first Example
{% highlight python %}
class Example:
    def __init__(self, items=[]):
        self.items = items

    def add_item(self, item):
        self.items.append(item)
{% endhighlight %}

I set the default value of items to be an empty list. Now, let's create two instances of Example and add items to each instance:

{% highlight python %}
# Create instance 1
obj1 = Example()
obj1.add_item(1)

# Create instance 2
obj2 = Example()
obj2.add_item(2)

print("Instance 1 items:", obj1.items)
print("Instance 2 items:", obj2.items)
{% endhighlight %}

You might expect the output to be:

```
Instance 1 items: [1]
Instance 2 items: [2]
```
However, the actual output will be:

```
Instance 1 items: [1, 2]
Instance 2 items: [1, 2]
```

This happens because both obj1 and obj2 share the same default list object. When you modify obj1.items by calling add_item(1), you're actually modifying the default list object, which is also referenced by obj2.items.
To avoid this behavior and ensure each instance has its own independent list, you should initialize the mutable attribute inside the __init__ method like so:

{% highlight python %}
class Example:
    def __init__(self, items=None):
        self.items = items or []

    def add_item(self, item):
        self.items.append(item)
{% endhighlight %}

With this modification, each instance will have its own empty list if no list is provided during initialization, preventing the sharing of mutable objects between instances.
