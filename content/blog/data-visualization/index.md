---
title: "📈 How I Visualize My Knowledge Graph"
summary: "Using Obsidian and data visualization to map out my research."
date: 2026-11-21
authors:
  - me
tags:
  - Obsidian
  - Data Visualization
  - Knowledge Graph
status: published
---

<video width="100%" controls autoplay loop muted>
  <source src="demo.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

{{< toc mobile_only=true is_open=true >}}

## 1. Introduction

During my PhD journey, I read hundreds of papers, completed many projects, and finally finished my dissertation. But when you have thousands of documents, how do you keep track of how they connect? 

Text is linear. But knowledge is a network. 

I have always been a visual thinker. When I looked at my folder, I just saw a huge pile of files. To truly understand their connections, I turned my notes into an interactive knowledge graph. 

In this post, I want to share a short video of how it works, the logic behind it, and how it helped me see the big picture of my work.

## 2. The Graph System

As you can see in the video, the graph isn't just a random collection of dots. I created a strict system using color-coded tags. There are four kinds of nodes:

- 🟣 **Purple (`#output`)**: The core of my PhD. This includes my dissertation and published papers. It is the largest node because it has the most connections.
- 🔵 **Blue (`#literature`)**: The research papers, textbooks, and books I read.
- 🟢 **Green (`#engineering`)**: The tools, systems, and manuals I actually used (like CUDA, XACML, or SELinux).
- 🟡 **Yellow (`#theory`)**: The math and computer science ideas behind it all: from Graph Theory and Computational Learning Theory to Policy Mining.

## 3. Why Visualize

When you look at the graph, you realize that a PhD isn't just a single paper. My thesis sits in the center, pulling in the yellow theory nodes, which then connect to the blue literature nodes. 

A larger node means more connections (links going in and out). By looking at this, I could quickly see which ideas were the "pillars" of my research and which papers were dead ends. 

It also helped me find missing connections. If a literature note had no link to an engineering tool, maybe I missed a chance to apply it. Visualizing my notes didn't just look nice; it actually changed how I thought about my own research.

## 4. The Tools I Used

To make this work easily, I relied on a few Obsidian tools and a specific workflow for my references:

1. **Tags and Properties**: I used specific tags (like `#output` and `#theory`) to sort every note. 
2. **AI and DBLP for References**: I needed a `Reference.bib` file to manage all the authors and titles. To save time, **I used AI to generate this file**. However, I did not trust it blindly. I used **DBLP** (dblp.uni-trier.de) to verify the correctness of the bibliography. DBLP is a free computer science database that provides open information on major journals and proceedings. AI generated the data quickly, but DBLP made sure it was 100% accurate.
3. **Graph View & 3D Graph**: These plugins read my tags and draw the network you see in the video. The 3D Graph is very helpful because it hides empty notes, so the picture stays clean.
4. **Dataview**: This plugin helps me search and sort my notes automatically into tables. 
5. **Omnisearch**: This helps me find notes instantly by ranking the titles higher than the text.

## 5. Conclusion

Data visualization isn't just about making pretty pictures. It's about making the invisible visible. 
A knowledge graph is a map of your own mind. It shows you not just what you know, but how everything connects. 
If you have too many notes and feel lost, I highly recommend building a graph. You might be surprised by what you find.

## License
Released under the [MIT](https://github.com/HugoBlox/kit/blob/main/LICENSE.md) license.
