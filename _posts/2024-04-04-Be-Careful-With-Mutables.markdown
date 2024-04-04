---
layout: post
title:  "Python: Be careful using mutables in python classes"
date:   2024-04-04 17:18:00 +0200
categories: blog
---

# Python: Be careful using mutables in python classes
Object-oriented programming (OOP) is renowned for its efficiency and intuitive approach to managing data structures. By creating objects with their own encapsulated data, functions, and logic, developers can build robust and modular systems. Recently, while developing my own parser in Python, I encountered a fascinating quirk when working with mutable data types like lists.

I discovered that the mutability of certain data types could lead to unexpected results. In this post I will cover an example of a class that keeps a list of items. 

## The first Example
{% highlight python %}
class Example:
    def __init__(self, items=[]):
        self.items = items

    def add_item(self, item):
        self.items.append(item)
{% endhighlight %}

To account for the possibility that the class is instantiated before the list of items is known, I want to set a default value of items to be an empty list. This is good practice as it provides some documentation on what the class expects as parameters. Now, let's create two instances of Example and add items to each instance:

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
