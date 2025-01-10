---
layout: post
title: "Visualizing Pi with the Monte Carlo Method"
date: 2025-01-10
categories: [projects, visualization, math]
tags: [C++, SDL2, Emscripten, WebAssembly, Monte Carlo]
---
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>  
<script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>  

Approximating Pi has fascinated mathematicians and programmers alike for centuries. One particularly elegant approach is the **[Monte Carlo method]**, which uses randomness and geometry to estimate this famous constant.
<div class= "emscripten-border" id="wasm-container" style="text-align: center;">
    <p id="loading-text">Loading the Monte Carlo Pi Visualizer...</p>
    <canvas class="emscripten" id="canvas"  width="400" height="480" oncontextmenu="event.preventDefault()" ></canvas>
</div>
Recently, I started tinkering with **[SDL2]** — a graphics rendering library written in C. Rendering visuals is an important part of understanding how an algorithm or dataset behaves. It also provides an intuitive way to create user interaction. While understanding graphics rendering concepts can be challenging, it is extremely satisfying once mastered. 

In this post, I'll showcase a program I built with **[SDL2]** to visualize how the [Monte Carlo method] works. The program supports both native builds and WebAssembly, allowing it to run directly in your browser. Users can interact by clicking with the mouse — pausing and resetting the simulation.

---

### Interacting with the Visualization

The Monte Carlo method is a probabilistic technique that relies on random sampling. Here's a breakdown of the steps:

1. **Generate Random Points**: Points are randomly distributed in a square.
2. **Check for Circle Inclusion**: For each point, determine if it lies inside a circle inscribed within the square.
3. **Estimate Pi**: The ratio of points inside the circle to the total number of points is used to approximate Pi:

   $$
   \pi \approx 4 \times \frac{\text{Points Inside Circle}}{\text{Total Points}}
   $$

This technique leverages the geometry of the circle and square, and as more points are sampled, the approximation improves.

---

### How Does the Monte Carlo Method Approximate Pi?
Once the program is loaded, you will see a square with random points being generated. Each point is either inside the circle (blue) or outside (red). The ratio of blue points to the total points approximates Pi in real time.


You can:
- **Pause the Simulation**: Left-clicking pauses if active and activates if paused. 
- **Reset the Simulation**: Right-click while in paused state to reset the simulation. 

---
### Building the Program

The program is written in **C++** using **SDL2** for graphics and text rendering. It supports both desktop builds with **CMake** and WebAssembly builds using **Emscripten**. Source files and further instruction for building the program can be found in my [Github-repo].

[SDL2]: https://www.libsdl.org
[Monte Carlo method]: https://en.wikipedia.org/wiki/Monte_Carlo_method
[Github-repo]: https://github.com/tomkalervo/my_wasm/tree/main/monteCarloPi

<script>
var Module = {
    preRun: [],
    postRun: [() => {
        // Remove the loading text once the module is ready
        var loadingText = document.getElementById('loading-text');
        if (loadingText) {
            loadingText.remove();
        }
    }],
    canvas: (() => {
        var canvas = document.getElementById('canvas');

        // Handle WebGL context loss gracefully
        canvas.addEventListener("webglcontextlost", (e) => {
            alert('WebGL context lost. You will need to reload the page.');
            e.preventDefault();
        }, false);

        return canvas;
    })(),
    locateFile: function(path) {
        if (path.endsWith('.wasm') || path.endsWith('.data')) {
            return '/assets/wasm/mcp/' + path;
        }
        return path;
    }
};
</script>

<script src="/assets/wasm/mcp/mcp.js"></script>
