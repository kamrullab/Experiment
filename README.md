## Experiment 7: Shortest Path Algorithm (Dijkstra’s Algorithm)

**Objective:**  
To implement Dijkstra’s Algorithm in C for finding the shortest path from a source vertex to all other vertices in a weighted graph.

---

### Algorithm Principles:

Dijkstra's Algorithm is a **greedy algorithm** that solves the single-source shortest path problem for graphs with non-negative weights. It iteratively selects the unvisited vertex with the smallest tentative distance, updates the distances of its neighbors, and marks it as visited. The algorithm continues until all vertices have been processed.

Key steps:
1. Initialize the distance of all vertices to infinity (`INT_MAX`), except the source vertex, which is set to `0`.
2. Use a set (or an array) to keep track of visited vertices.
3. Update the distances of adjacent vertices if the sum of the current vertex's distance and the edge weight is less than the known distance.
4. Repeat the process until all vertices are visited.

---

### Example:

**Graph:**
```
       0 --(1)--> 1 --(4)--> 2
       |         /
      (4)       (2)
       |       /
       3 --(3)
```

**Adjacency Matrix Representation:**
```
   0   1   2   3
0 [0,  1,  0,  4]
1 [1,  0,  4,  2]
2 [0,  4,  0,  3]
3 [4,  2,  3,  0]
```

**Shortest Paths from Vertex 0:**  
- To Vertex 1: 1  
- To Vertex 2: 3  
- To Vertex 3: 4  

---

### Code:

```c
#include <stdio.h>
#include <limits.h>

#define V 4  // Number of vertices in the graph

int findMinDistance(int dist[], int visited[]) {
    int min = INT_MAX, minIndex = -1;
    for (int i = 0; i < V; i++) {
        if (!visited[i] && dist[i] <= min) {
            min = dist[i];
            minIndex = i;
        }
    }
    return minIndex;
}

void dijkstra(int graph[V][V], int src) {
    int dist[V], visited[V] = {0};
    for (int i = 0; i < V; i++) dist[i] = INT_MAX;
    dist[src] = 0;

    for (int count = 0; count < V - 1; count++) {
        int u = findMinDistance(dist, visited);
        visited[u] = 1;

        for (int v = 0; v < V; v++) {
            if (!visited[v] && graph[u][v] && dist[u] != INT_MAX && dist[u] + graph[u][v] < dist[v]) {
                dist[v] = dist[u] + graph[u][v];
            }
        }
    }

    printf("Vertex\tShortest Distance from Source %d\n", src);
    for (int i = 0; i < V; i++) {
        printf("%d\t%d\n", i, dist[i]);
    }
}

int main() {
    int graph[V][V] = {
        {0, 1, 0, 4},
        {1, 0, 4, 2},
        {0, 4, 0, 3},
        {4, 2, 3, 0}
    };

    dijkstra(graph, 0);  // Compute shortest paths from vertex 0
    return 0;
}
```

---

### Complexity Analysis:

- **Time Complexity:**  
  - Using adjacency matrix: `O(V^2)` (due to `V` iterations for `findMinDistance`).
  - Using adjacency list with a priority queue (not shown here): `O(E log V)`.

- **Space Complexity:** `O(V)` for storing distances and visited vertices.

---

### Observations:

1. The algorithm successfully computes the shortest distances from the source vertex to all other vertices in the graph.  
2. It works well for graphs with non-negative edge weights.  
3. The algorithm's performance is directly influenced by the number of vertices (`V`) and edges (`E`).

---

### Conclusion:

Dijkstra's Algorithm is an efficient and widely-used algorithm for solving single-source shortest path problems in graphs with non-negative edge weights. Its greedy approach ensures that the shortest path for each vertex is computed optimally. Applications include GPS navigation, network routing, and resource optimization problems.
