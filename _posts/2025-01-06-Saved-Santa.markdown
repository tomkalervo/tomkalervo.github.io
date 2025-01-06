---
layout: post  
title: "Santa is Saved: I Completed AOC 2025"  
date: 2025-01-06 16:39:00 +0200  
categories: blog  
---

This December, I finally had the time (and patience) to see it through and complete all 25 days of coding challenges.  
Advent of Code (AOC) is a website that celebrates its 10th anniversary this year. Each year, starting on December 1st and running through December 25th, a new challenge is published daily.  
Some are harder than others, and in general, the difficulty increases with each passing day.


You can explore my full solutions to Advent of Code 2024 on GitHub: [Advent of Code 2024](https://github.com/tomkalervo/adventofcode2024/tree/main).  

![AOC2024](/assets/img/aoc_complete_2024.png)

## A Bumpy Road  

Thanks to my fair share of knowledge in graph algorithms, I managed to tackle most of the harder challenges with relative ease. 
However, the ones I struggled with the most were those that required closer inspection and analysis of the input data.
My usual approach is to identify and implement a general algorithm to solve the problem. But this year, I learned the importance of examining the data itself, not just the problem statement.

These challenges took more time to solve, but they better reflect real-world scenarios, where understanding your data is critical before jumping to a solution.  

## Day 25 Challenge: Matching Keys to Locks  

The Day 25 challenge was to determine how many keys could fit into each lock and return the total number of valid combinations. 
I represented both keys and locks as arrays, each containing five integers to describe the pin lengths at different positions.

For example: If a lock has a pin length `x` at index `i`, then only keys with a pin length `y` where `0 <= y <= 5-x` at index `i` would fit.

To optimize the solution, I pre-sorted the keys using a lambda function, allowing for early termination when encountering mismatches. While this optimization didn't change the time complexity, it was satisfying to implement.  
                                  

Using C++ proved to be a great decision—not only did it enable me to solve the challenges effectively, but it also pushed me to learn several new techniques.  
Over the course of these challenges, I transitioned from a clear OOP approach to a more sequential, functional style with static functions and lambda helpers.  

---

[AOC]: https://adventofcode.com

Here’s the code:  

{% highlight cpp %}
static uint64_t combinations(const vector<array<uint32_t, 5>> &locks,
                             const vector<array<uint32_t, 5>> &keys) {
  // Perform a sort on locks and keys to improve comparison efficiency
  int idx = 0;
  auto arraySort = [&idx](array<uint32_t, 5> a, array<uint32_t, 5> b) {
    return a[idx] < b[idx];
  };

  auto sort_keys = keys;
  auto sort_locks = locks;
  vector<array<uint32_t, 5>> matched;

  uint64_t combos = 0;
  for (auto lock : locks) {
    for (idx = 0; idx < 5; idx++) {
      sort(sort_keys.begin(), sort_keys.end(), arraySort);
      sort(sort_locks.begin(), sort_locks.end(), arraySort);
      for (int j = 0; j < sort_keys.size(); j++) {
        if (lock[idx] + sort_keys[j][idx] > 5) {
          break;
        }
        matched.push_back(sort_keys[j]);
      }
      sort_keys = matched;
      matched.clear();
    }
    combos += sort_keys.size();
    sort_keys = keys;
  }
  return combos;
}
{% endhighlight %}  

