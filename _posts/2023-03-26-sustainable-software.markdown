---
layout: post
title:  "Software is not sustainable"
date:   2023-03-26 16:30:46 +0200
categories: blog
---

I own a three year old `?smart?`-tv. When watching streaming-services we usually go through an app from the `?smart?`-system built into the tv. A few months ago, some apps needed updates and one of them said it would no longer support my tv model. Some light rage came and passed. Thankfully it is just as easy to stream through our smart devices directly to the tv. But it got me thinking, what is the reason that software developers stop supporting three year old devices? I have the same experience regarding my ipad, it is however much older. But still very functional.

I recently studied a course in sustainable development and innovation. It was on some levels an eye-opener. I will try and summarize important aspects regarding sustainability in software development.

# Sustainability as such
The aspects of sustainability should always be present during development, even software. Defining sustainability is a chapter on its own, the [Brundtland] report from 1987 states that:
> Sustainable development is development that meets the needs of the present without compromising the ability of future generations to meet their own needs. 
It is quite obvious that we today, and for quite a while, have been exploiting our planet more than it can handle.

# Hardware in general
Lice-Cycle Assessment, (LCA),  is a common framework, standardized by ISO, to measure and define the sustainability of products and services. Hardware has most of its environmental impact during production and disposal. For example harvesting of rare minerals, factory-related emissions and handling of electronic waste have great impact on sustainability. It is almost always the case that the hardware-products do not live long enough to be considered sustainable. 

# Software in specific
It is not as easy to picture the impact on sustainability from software. What first comes to mind is energy-efficient code (or algorithm-design). This is true, the more frequent a software is used, the more energy there is to save with clever algorithms and design. If heavy on network-traffic, a cache might decrease network traffic, and thus also energy consumption. 

But the aspects I find most interesting is to develop code that prolongs the life of hardware. Let?s not forget that it is the development of new software that makes \?old\? hardware obsolete. Far too often does new updates stop supporting older hardware. This creates an unnecessary consumption of hardware - directly shortens the lifespan of products. Here lies a major responsibility for software developers to continue to support older devices.

[Brundtland]: https://en.wikipedia.org/wiki/Brundtland_Commission