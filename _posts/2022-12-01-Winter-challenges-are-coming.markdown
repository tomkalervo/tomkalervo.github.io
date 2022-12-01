---
layout: post
title:  "Winter challenges are coming"
date:   2022-12-01 20:38:46 +0200
categories: blog
---
Today is the first of December which means that today is the start of [Advent of Code]. A yearly coding challenge that releases one new challenge each day during December until Christmas. This is a great opportunity to either learn a new programming language or refresh one you haven't used in a while. I decided on the latter and refreshed my Elixir skills. 

Today's challenge involved some string operations - which comes very natural in Elixir. There were some quirks however. The sample input was the following:
```
1000
2000
3000

4000

5000
6000

7000
8000
9000

10000
````
The goal was to summarize the contiguous values and find the largest sum (which is 24000). This was achieved with a couple of pipes:
{% highlight elixir %}
String.split(In.put, ~r/(\n)/)
    |> List.foldl([0], fn(x,[h|t])->
      if x == "" do
        [0,h|t]
      else
        [h+String.to_integer(x)|t]
      end
    end)
    |> List.foldl(0, fn(x, max)-> if x > max, do: x, else: max end)
{% endhighlight %}

In.put represents the input formatted as a string. The key to split the string was to use a [regex]. Without it we would lose the blank lines. Now we get a list where each line is an element. Here on we can pipe the list through to List.foldl/3 and calculate the sum of contiguous values. From here on we have a new list where we need to find the highest value. It is done by piping it through another List.foldl/3 that accomplishes this.

[Advent of Code]: https://adventofcode.com
[regex]: https://hexdocs.pm/elixir/1.13/Regex.html