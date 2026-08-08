# Assignment 2 - Array, Tree, Graph Problem Statement

---

## Problem 1: Tree of Trusted Servers

### Problem Description
A multinational organization maintains its internal communication infrastructure in the form of a tree, where each server is represented by a node, and each communication link is represented by an edge. The root server (Server 1) is the central authentication server. Every server stores a unique security key represented by an integer. During periodic audits, the organization wants to verify whether the communication path between the root server and every other server satisfies a security policy.

A server is considered trusted if the XOR of all security keys on the path from the root server to that server is greater than or equal to a given threshold K. Your task is to determine how many servers in the network are trusted. The root server is also included in the evaluation.

### Input Format
- **First line:** `N K`
- **Second line:** `N` integers (security keys)
- **Next N-1 lines:** `u v` (tree edges)

### Output Format
- Print the number of trusted servers.

### Constraints
- $1 \le N \le 10^5$
- $0 \le K \le 10^9$
- $0 \le \text{Key} \le 10^9$
- Input graph is a tree

### Sample Input
```text
7 5
3 6 2 7 1 4 5
1 2
1 3
2 4
2 5
3 6
3 7
```

### Sample Output
```text
4
```

### Explanation
Compute the XOR value along the path from the root (Server 1) to every server. Count the servers whose path XOR is at least $K$.

---

### Solution (Python)
```python
import sys

def solve():
    input_data = sys.stdin.read().split()
    if not input_data:
        return

    # Read N and K
    N = int(input_data[0])
    K = int(input_data[1])

    # Security keys for servers (1-indexed for convenience)
    keys = [0] + [int(x) for x in input_data[2:2 + N]]

    # Read tree edges
    adj = [[] for _ in range(N + 1)]
    idx = 2 + N
    for _ in range(N - 1):
        u = int(input_data[idx])
        v = int(input_data[idx + 1])
        adj[u].append(v)
        adj[v].append(u)
        idx += 2

    # Iterative DFS/BFS to traverse tree from root (Server 1)
    # Stack stores tuples of: (current_node, parent_node, current_path_xor)
    stack = [(1, 0, keys[1])]
    trusted_count = 0

    while stack:
        u, parent, curr_xor = stack.pop()

        # Check if the cumulative path XOR satisfies the security threshold K
        if curr_xor >= K:
            trusted_count += 1

        # Traverse neighbors
        for v in adj[u]:
            if v != parent:
                stack.append((v, u, curr_xor ^ keys[v]))

    print(trusted_count)

if __name__ == '__main__':
    solve()
```

---

## Problem 2: Emergency Route Validation

### Problem Description
A country's transportation department models its highway system as a connected undirected graph. Each city is represented by a vertex, while highways are represented by edges. During emergencies, rescue teams need to travel from the capital city (City 1) to all other cities. However, not every city is considered safely reachable because some routes may contain too many intermediate cities.

A city is called efficiently reachable if the length of the shortest path from City 1 to that city is less than or equal to $D$ roads. Determine the total number of efficiently reachable cities, including the capital. Unreachable cities are not counted.

### Input Format
- **First line:** `N M D`
- **Next M lines:** `u v` (roads)

### Output Format
- Print the number of efficiently reachable cities.

### Constraints
- $1 \le N \le 10^5$
- $0 \le M \le 2 \times 10^5$
- $0 \le D \le N$
- No self-loops

### Sample Input
```text
7 8 2
1 2
1 3
2 4
2 5
3 6
6 7
5 7
4 6
```

### Sample Output
```text
6
```

### Explanation
Run BFS from City 1 to compute the shortest distance to every city. Count cities whose distance is at most $D$.

---

### Solution (Python)
```python
import sys
from collections import deque

def solve():
    input_data = sys.stdin.read().split()
    if not input_data:
        return

    # Read N, M, D
    N = int(input_data[0])
    M = int(input_data[1])
    D = int(input_data[2])

    # Build adjacency list for undirected graph (1-indexed)
    adj = [[] for _ in range(N + 1)]
    idx = 3
    for _ in range(M):
        u = int(input_data[idx])
        v = int(input_data[idx + 1])
        adj[u].append(v)
        adj[v].append(u)
        idx += 2

    # BFS starting from Capital (City 1)
    visited = [False] * (N + 1)
    queue = deque([(1, 0)])  # (city, distance)
    visited[1] = True
    
    reachable_count = 0

    while queue:
        u, dist = queue.popleft()
        reachable_count += 1

        # If current distance is already D, its neighbors will have distance D + 1 > D
        if dist < D:
            for v in adj[u]:
                if not visited[v]:
                    visited[v] = True
                    queue.append((v, dist + 1))

    print(reachable_count)

if __name__ == '__main__':
    solve()
```
