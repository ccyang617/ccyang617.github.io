---
layout: essay
type: essay
title: "Design Patterns: A Product of Dynamic Programming"
# All dates must be YYYY-MM-DD format!
date: 2024-04-25
published: true
labels:
  - Software Engineering
  - Learning
  - Web Dev
  - UI
  - Algorithms
  - Data Structures
---

<img width="303px" class="rounded float-start pe-4" src="../img/bootstrap5-icon.jpg">

<h1>Common Data Patterns Seen In Algorithms</h1>
<h4>Dijkstra's Algorithm</h4>
Throughout my time at the University of Hawaii at Manoa, I have been introduced to various approaches to problem solving. One of these approaches is dynamic programming, the practice of breaking a problem down into smaller sub-problems to create the solution. Dynamic programming has been the focus of my semester this year as I currently enrolled in the Data Structures & Algorithms course. The concept of dynamic programming allows software engineers to identify recurring patterns in problem. When we looking at Dijkstra's algorithm to find the shortest path in a tree, we can see an example of a design pattern used for sorting, the min-heap. Dijkstra's algorithm utlizes this data pattern for sorting because it efficient and utilizes the optimum time complexity for this case. This is often the case with common data patterns we encounter in school as the most efficient practices are typically standard. Below I have included an example of Dijkstra's Algorithm in pseudocode.

```
DIJKSTRA(G, s): //Builds a shortest path tree from node s
for all v ∈ V:
  dist[v] = ∞
  prev[v] = null

dist[s] = 0
S={} // keep track of visited nodes, nodes in the shortest path tree
Q = MakeHeap(V) // use priority queue, e.g, min heap

while !IsEmpty(Q):
  u = ExtractMin(Q)
  S = S U {u}
  for all edges (u, v) ∈ E:
    if dist[v] > dist[u] + w(u, v):
    dist[v] = dist[u] + w(u, v)
    prev[v] = u
    DecreaseKey(Q, v, dist[v]) //update value in the heap
```
<h4>Object-Oriented Data Structures</h4>
In many other coding algorithms there are examples of the use of commonly practiced design patterns. But there are other common patterns that are apparent as well. A common practice that can be seen throughout many designs is object-oriented programming data structures. This common practice is used because it can help optimize the way data is passed throughout a system.

<h4>Everything Is For Optimization</h4>
The reason we see so many common design patterns throughout various code is because these design patterns are often the most optimized solutions available. Functions that sort a list or traverse a tree are going to be needed in almost everythign we do. Reading data efficiently allows programs to run dynamically and update based on various situations. This is why we notice the difference between companies with many resources and ones with few. The ability to optimize is reflected in the quality of the product. When applications and programs are fast and responsive the difference is very noticeable. This helps to illustrate how important the use of these data patterns are and how important it is for students to pay attention. Noticing these trends will only help facilitate us to reach our coding potential.
