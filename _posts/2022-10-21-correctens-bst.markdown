---
layout: post
title:  "Proving correctnes of binary search"
date:   2022-10-21 15:55:46 +0200
categories: blog
---

### Validation and verification
One important aspect of algorithm design is the abillity to prove that your algorithm behaves as intended. Usually we run a variety of tests to verify this. This is almost, always needed for any larger or more complex systems. But if we want to validate the program, we have to prove its correctness. This is based on math and logical reasoning. This can be a bit tricky so you need to start att the lower levels. Validating the different parts a larger program is a good approach. It helps to make sure each part works as intended.

### An algoritm
I recently had to implement a variant of a binary search when designing an algoritm by reduction. Simply put, the problem could be solved by binary search if I preprocessed the data. I had a value, *x*, and needed to know the index of the value, *y*, from a sorted array of numbers. The condition was that *y* needed to be the largest number in the array that was less than *x*. If *y* was found at Array[i], then *i* is the return-value. I had three conditions on the input: The first is that Array[0] = 0, the 0th index in the array has the value 0. The second is that all values in Array[0..n] are distinct (no two are the same). The third is that *x* > 0.

{% highlight pseudo %}
function find_y(Array[0..n], x)
    left <- 1
    right <- n
    // Invariant: Array[left - 1] < x AND Array[right + 1] > x
    while left <= right do
        mid <- floor( (left+right) / 2 )
        if Array[mid] > x then
            right <- mid - 1
        else if Array[mid] < x then
            left <- mid + 1
        else
            return mid - 1
    return left - 1
{% endhighlight %}

Quite simple right? We have an array, start at the middle and compare our *x* value. If *x* is found, we can return the index below *mid*. If *x* is not found we either continue our search in the left half or the right half (thus decreasing our search area by one half). If *x* is not found in Array[1..n] we still want to get the index of a value *y* as described above. The code says that this value, after the loop is complete, is to be found at the index of *left* - 1. How can we know that? 

### An invariant-based proof
If you can prove that the invariant is true throughout the loop and that the invariant leads to the intended output - then you are set. An invariant is really helpful when designing loops. [yourbasic.org] even claim that it gives you superpowers! It really helps you keep you head straight when doing the design for a loop. 
The invariant is a condition that is true when entering the loop. It is also true after every iteration of the loop. Lastly, but not least, it is true after the loop. 

{% highlight pseudo %}
Invariant: Array[left - 1] < x AND Array[right + 1] > x
{% endhighlight %}

- Before entering the loop we see *left* is set to 1, thus *left* - 1 = 0. From the pre-conditions we have Array[0] = 0 and *x* > 0. Since *right* is set to *n*, *right* + 1 is outside the range and can be viewed as infinite. So the invariant is true before the loop.
- Inside the loop we have three cases. 
    1. If *x* happens to be found, it is trivial to se that the index *mid* - 1 holds the value *y* we are searching for. This is returned which terminates the function. This is the same as changing the value of *left* <- *mid* (and break the loop - but not needed for the correctness).
    2. If *x* is less than Array[mid] we continue our search among the half that contains the lesser values. We set *right* <- *mid* - 1, this means that the value at Array[right+1] is the value we compared with *x*. The invariant is still true.
    3. If *x* is greater than Array[mid] we continue our search among the half that contains the greater values. We do update *left*. We set *left* <- *mid* + 1, this means that the value at Array[left-1] is the value we compared with *x*. The invariant is still true.
- Termination: If the value *x* is not found we will have a combination of alternative 1 and 2. The area to search is at least decreased by 1 in every iteration. This means that eventually *left* will become greater than *right*. Thus the loop will terminate. 

- Invariant: We have the same reasoning as inside the loop. Since the invariant was true in all iterations inside the loop it is still true after. 

#### Conclusion
After the loop is terminated we have the following facts:
- The invariant is true
- *left* > *right*
- *x* did not exists in the array

Actuallay, *left* > *right* means that *left* is equal to *right* + 1. From Array[right+1] > *x* we get Array[left] > *x*. Together with Array[left-1] < *x* we see that *x* is enclosed by *left* and *left*-1. We can say for certain that the value we sought is found at index *left*-1.


[yourbasic.org]: https://yourbasic.org/algorithms/loop-invariants-explained/