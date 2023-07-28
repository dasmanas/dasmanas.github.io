---
layout: post
title:  "In Large Dataset Detect Cycles in a DAG!"
date:   2023-07-27 19:35:00 +0800
categories: bigdata dag cycle spark spark-graph
---
In a directed acylic graph, cycle is not expected. If it is there, we can find it using Depth First Traversal Technique. In a recursive call, if we keep track of visited nodes, we will able to identify if there is any cycle exist. But in landscape of large scale data using DFS could be challenging as we have to keep maininting the visited nodes array and need to pass to the next node. It might cause a high memory overhead to the solution. The task could be solved in a bit different approach using [Rocha–Thatte cycle detection algorithm](https://en.wikipedia.org/wiki/Rocha–Thatte_cycle_detection_algorithm).

```mermaid
graph LR
A -->|A| B
B -->|B| C
C -->|C| E
C -->|C| D
D -->|D| B
```