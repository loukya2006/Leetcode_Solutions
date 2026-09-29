# Leetcode_Solutions

# Path With Minimum Effort

# Problem

Given a 2D grid `heights`, each cell represents the height of a location. Starting from the top-left cell `(0, 0)`, we need to reach the bottom-right cell.

Movement is allowed in four directions:

* Up
* Down
* Left
* Right

The effort of a path is defined as the **maximum absolute difference in height between two consecutive cells** along that path.

The objective is to find the path requiring the minimum possible effort.

---

# Problem Statement

Given a matrix of heights, find the minimum effort required to travel from the top-left cell to the bottom-right cell.

For every move between two adjacent cells:

```text
Difference = |height of current cell - height of next cell|
```

The effort of the complete path is:

```text
Maximum of all differences along the path
```

Return the minimum possible effort.

---

# Example

# Input

```text
heights = [[1,2,2],
           [3,8,2],
           [5,3,5]]
```

# Output

```text
2
```

# Explanation

One possible path is:

```text
1 → 3 → 5 → 3 → 5
```

The differences are:

```text
|1 - 3| = 2
|3 - 5| = 2
|5 - 3| = 2
|3 - 5| = 2
```

Therefore, the maximum difference is `2`.

Hence, the minimum effort is:

```text
2
```

---

# Approach

This problem can be solved using **Dijkstra's algorithm**.

Each cell in the matrix is treated as a node in a graph.

The four possible movements from a cell represent edges.

The cost of moving from one cell to another is:

```text
abs(current height - next height)
```

Unlike normal Dijkstra's algorithm, we do not add the edge costs.

Instead, we keep track of the maximum difference encountered so far:

```python
new_effort = max(current_effort, difference)
```

If this new effort is smaller than the previously calculated effort for the neighboring cell, we update it.

---

# Algorithm

1. Create a `dist` matrix to store the minimum effort required to reach every cell.
2. Set the effort of the starting cell `(0,0)` to `0`.
3. Create a `visited` matrix.
4. Find the unvisited cell having the smallest effort.
5. Mark the cell as visited.
6. Check its four neighboring cells.
7. Calculate the height difference between the current cell and neighbor.
8. Calculate the new effort using:

```text
new_effort = max(current_effort, height_difference)
```

9. Update the neighbor if the new effort is smaller.
10. Continue until the destination is reached.

---

# Source Code

```python
class Solution:
    def minimumEffortPath(self, heights):
        m = len(heights)
        n = len(heights[0])

        dist = [[float('inf')] * n for _ in range(m)]
        visited = [[False] * n for _ in range(m)]

        dist[0][0] = 0

        for _ in range(m * n):

            min_effort = float('inf')
            r = c = -1

            for i in range(m):
                for j in range(n):
                    if not visited[i][j] and dist[i][j] < min_effort:
                        min_effort = dist[i][j]
                        r, c = i, j

            visited[r][c] = True

            if r == m - 1 and c == n - 1:
                return dist[r][c]

            for dr, dc in [(1, 0), (-1, 0), (0, 1), (0, -1)]:
                nr = r + dr
                nc = c + dc

                if 0 <= nr < m and 0 <= nc < n:

                    diff = abs(heights[r][c] - heights[nr][nc])

                    new_effort = max(dist[r][c], diff)

                    if new_effort < dist[nr][nc]:
                        dist[nr][nc] = new_effort

        return 0
```

---

# Time Complexity

There are `rows × columns` cells.

For every cell, we search the entire matrix to find the unvisited cell with minimum effort.

Therefore:

```text
Time Complexity: O((rows × columns)²)
```

The `dist` and `visited` matrices require:

```text
Space Complexity: O(rows × columns)
```

---

# Test Cases

# Test Case 1

```text
Input:
[[1,2,2],
 [3,8,2],
 [5,3,5]]

Output:
2
```

# Test Case 2

```text
Input:
[[1,2,3],
 [3,8,4],
 [5,3,5]]

Output:
1
```

# Test Case 3

```text
Input:
[[1,2,1,1,1],
 [1,2,1,2,1],
 [1,2,1,2,1],
 [1,2,1,2,1],
 [1,1,1,2,1]]

Output:
0
```

---

# Key Concept

The important idea in this problem is that the path cost is **not the sum of edge differences**.

For example:

```text
Path differences = [1, 3, 2, 1]
```

The effort is:

```text
max(1, 3, 2, 1) = 3
```

Therefore, during Dijkstra's algorithm we use:

```python
new_effort = max(current_effort, difference)
```

instead of:

```python
new_effort = current_effort + difference
```

---

# Result

The program successfully finds the minimum possible effort required to travel from the top-left cell to the bottom-right cell using Dijkstra's algorithm without using a priority queue.

# LeetCode

**Problem:** 1631. Path With Minimum Effort



**Topic:** Graphs, Dijkstra's Algorithm, Matrix
