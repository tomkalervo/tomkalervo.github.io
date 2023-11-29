---
layout: post
title:  "AOC - Preparing Python"
date:   2023-08-29 21:13:46 +0200
categories: blog
---

The temperature has dropped below zero, white gleaming snow lights up the winter darkness and Kris Kringle is working hard at his workshop. This weekend the yearly [Advent of Code] launches - providing a new code challange each day of Christmans. This year I am to give Python a shot. Being a somewhat beginner at Python I hope that learning and using python might speed up the coding process - at least that is my current view of python; a quick way to prototype code ideas.

To prepare I created a code snippet for reading files as input. Since all challanges involve input that can be quite large, it is almost always easiest to put it into a textfile and input that to the program solver. Here is what I came up with:

{% highlight python %}
def printit(x):
  print("input: ", x)


x = input()
while x:
  printit(x)
  try : x = input()
  except EOFError: break
{% endhighlight %}

The purpose is to read line by line and do necessary string parsing. As of now I only print out each line. There are of course other options to chose from, such as picking up the filepath from command line arguments and then open a file stream. I have not yet decided the best (most time effecient) approach. 

[Advent of Code]: https://adventofcode.com