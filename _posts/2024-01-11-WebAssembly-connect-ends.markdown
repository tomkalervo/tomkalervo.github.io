---
layout: post
title:  "WebAssembly - Connecting Ends"
date:   2024-01-11 20:39:00 +0200
categories: blog
---
<script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>

I made my first WebAssembly implementation, exploring two fibonacci-calculating-functions. It can be viewed at [From C to Wasm, Fibonacci].

# Why I Wanted to Learn WebAssembly
For years I have been coding algorithms, solving small and not so small problems. Most often, these programs stay as local C, Java, or Elixir applications - Since there is no ambition to build a finished product around it. But some programs would be fun to make available, for other to test and tinker with.

I recently finished a course on coursera regarding [Discret Optimization]. In this course I learned new concepts that I build prototypes on. They can be found in my github-repo for the course. Once again I felt the urge to be able to display/make the concepts interactable - So I started looking into WebAssembly:

>WebAssembly (abbreviated Wasm) is a binary instruction format for a stack-based virtual machine. Wasm is designed as a portable compilation target for programming languages, enabling deployment on the web for client and server applications. [Source]

# Fibonacci in c
To start off I wrote two c-functions to calculate a fibonacci-value. Fibonacci is a squence of *n* integers. It may be defined by the following recursive relation: 

Basecase(s):
<div>
$$ n_{0} = 0 $$
$$ n_{1} = 1 $$
</div>

Recursive case:
<div>
$$ n_{i} = n_{i-1} + n_{i-2} $$
</div> 

So the first function implemented in C follows the recursive formula:

{% highlight c %}
long fib_recursive(int n) {
    if (n <= 0)
        return 0;
    else if (n == 1)
        return 1;
    else
        return fib_recursive(n-2) + fib_recursive(n-1);
}
{% endhighlight %}

This implementation is sometimes called the "naive" implementation. The reason beeing that it will result in an unnecessary exponential time complexity (as well as memory complexity) - each call results in two new calls where same fibonacci values will be calculated multiple times.

A more reasonable approach is to use dynamic progragramming - a technique that can be chosen for any recursive formula. It starts from bottom up, the basecases, and stores only the values needed to calculate the next value. This becomes clear in my second c-function for fibonacci-calculation:

{% highlight c %}
long fib_dynamic_programming(int n) {
    if(n <= 0)
        return 0;
    long f0 = 0;
    long f1 = 1;
    while (n-- > 1){
        long temp = f1 + f0;
        f0 = f1;
        f1 = temp;
    }
    return f1;
}
{% endhighlight %}

# c in Wasm
To compile the c-code into wasm I conveniently used Brew on my mac. This installed [Emscripten], that can be run from the terminal command emcc.

```zsh
tomkarlsson@Toms-MBP ~ %  emcc fib.c -o fib.js   
```

It will produce a `fib.wasm` file and a `fib.js` file. The `.js` contains javascript that loads the `.wasm` file. To fully understand how to link the .wasm with javascript I decided to write my own HTML with inline javascript. Due to the nature of the two C functions having different timecomplexity (linear vs exponential) I also added a javascript timing-snippet to display the time it took to execute each function.

The HTML code, as well as the C code, can be found in my github-repo [my_wasm/fib/] - But! The awesomeness of Wasm now also make this little project interactable, -> [From C to Wasm, Fibonacci] <-

[Source]: https://webassembly.org
[Discret Optimization]: https://www.coursera.org/learn/discrete-optimization
[Emscripten]: https://emscripten.org/
[my_wasm/fib/]: https://github.com/tomkalervo/my_wasm/tree/main/fib
[From C to Wasm, Fibonacci]: https://htmlpreview.github.io/?https://github.com/tomkalervo/my_wasm/blob/main/fib/fib.html