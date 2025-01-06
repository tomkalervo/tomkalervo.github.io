---
layout: post  
title: "WebAssembly - Connecting Ends"  
date: 2024-01-11 20:39:00 +0200  
categories: blog  
---

<script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>  
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>  

I created my first WebAssembly implementation, exploring two Fibonacci-calculating functions. You can check it out at [From C to Wasm, Fibonacci].  

## Why I Wanted to Learn WebAssembly  

For years, I’ve been coding algorithms and solving problems—ranging from small challenges to more complex ones.  
Most of these programs remain local C, Java, or Elixir applications since I usually have no ambition to build a complete product around them.  
However, some programs would be fun to share, allowing others to test and tinker with them.  

Recently, I completed a Coursera course on [Discrete Optimization], where I learned new concepts and built prototypes. These can be found in my GitHub repository for the course.  
Once again, I felt the urge to make these concepts interactive and accessible, which led me to explore WebAssembly:  

> **WebAssembly (Wasm)** is a binary instruction format for a stack-based virtual machine.  
> Wasm is designed as a portable compilation target for programming languages, enabling deployment on the web for both client and server applications. [Source]  

## Fibonacci in C  

To get started, I wrote two C functions to calculate Fibonacci numbers.  
The Fibonacci sequence is a series of integers defined by the following recursive relation:  

**Base cases:**  
<div>
$$ n_0 = 0 $$  
$$ n_1 = 1 $$
</div>  

**Recursive case:**  
<div>
$$ n_i = n_{i-1} + n_{i-2} $$
</div>  

The first function I implemented in C follows the recursive formula directly:  

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

This is often called the "naive" implementation because it results in unnecessary exponential time and memory complexity.  
Each function call spawns two new calls, leading to the same Fibonacci values being recalculated multiple times.  

A more efficient approach uses **dynamic programming**—a technique that works bottom-up from the base cases, storing only the values needed for the next calculation.  

This approach is demonstrated in my second C function:  

{% highlight c %}
long fib_dynamic_programming(int n) {
    if (n <= 0)
        return 0;
    long f0 = 0;
    long f1 = 1;
    while (n-- > 1) {
        long temp = f1 + f0;
        f0 = f1;
        f1 = temp;
    }
    return f1;
}
{% endhighlight %}  

## C in Wasm  

To compile the C code into WebAssembly, I used Homebrew on my Mac to install [Emscripten]. This toolchain provides the `emcc` command for compiling C/C++ to Wasm.  
```zsh
tomkarlsson@Toms-MBP ~ % emcc fib.c -o fib.js  
```
This command generates two key files:  

1. **`fib.wasm`**: The WebAssembly binary, which contains the compiled code.  
2. **`fib.js`**: A JavaScript file that acts as a bridge, loading and executing the `.wasm` file in a web environment.  

To better understand how to integrate `.wasm` with JavaScript, I decided to write a simple HTML file with inline JavaScript. This allowed me to experiment with linking the WebAssembly module and calling the exported functions directly from JavaScript.  

Since the two C functions have vastly different time complexities (linear vs exponential), I added a small timing script in JavaScript to measure and display the execution time of each function. This made it easier to observe the performance differences in real-time.  

The full HTML and C code are available in my GitHub repository: [my_wasm/fib/].  
But thanks to the power of WebAssembly, this project is now fully interactive! You can test it yourself here: [From C to Wasm, Fibonacci].  

[Source]: https://webassembly.org
[Discret Optimization]: https://www.coursera.org/learn/discrete-optimization
[Emscripten]: https://emscripten.org/
[my_wasm/fib/]: https://github.com/tomkalervo/my_wasm/tree/main/fib
[From C to Wasm, Fibonacci]: https://htmlpreview.github.io/?https://github.com/tomkalervo/my_wasm/blob/main/fib/fib.html
