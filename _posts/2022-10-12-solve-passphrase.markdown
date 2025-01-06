---
layout: post
title:  "Solving a passphrase the Elixir way"
date:   2022-10-12 20:26:46 +0200
categories: blog
---

Today at the thesis fair I talked to a super cool company called [WSI]. They develop custom solutions from sensor to applications, through all layers of a connected product.
Anyhow, they had a competition to reverse engineer a passphrase.
![passphrase](/assets/img/201210-phassphrase.jpg)
Here is a super fast way to get the secret code using Elixir:

{% highlight elixir %}
iex(1)> key = [3, 46, 44, 43, 33, 30, 46, 34, 38, 30, 10, 29, 25, -9, 33, 37,35]
[3, 46, 44, 43, 33, 30, 46, 34, 38, 30, 10, 29, 25, -9, 33, 37, 35]
iex(2)> {_, code} = List.foldl(key, {0,[]}, fn(x,{i,list}) -> {i+1,[x+i+64 | list]} end)
{17, 'stoDehTgnitcennoC'}
iex(3)> Enum.reverse(code)
'ConnectingTheDots'
{% endhighlight %}

Tadaa!


[WSI]: https://www.wsisweden.com
