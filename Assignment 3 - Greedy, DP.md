# Assignment 3 - Greedy, DP

---

## Problem 1: Scholarship Distribution (Greedy)

### Problem Description
A college has received a limited number of scholarships for students who participated in a coding competition. Every student has a minimum scholarship amount they are willing to accept. The college wants to award scholarships to as many students as possible without exceeding the available budget.

Each student can receive at most one scholarship, and the college can decide the order in which scholarships are awarded. Your task is to determine the maximum number of students who can receive scholarships while staying within the total budget.

### Input Format
- **First line:** `N B` (number of students and total budget)
- **Second line:** `N` integers representing the minimum scholarship required by each student.

### Output Format
- Print the maximum number of students who can receive scholarships.

### Constraints
- $1 \le N \le 100000$
- $1 \le B \le 10^9$
- $1 \le \text{Scholarship} \le 10^6$

### Sample Input
```text
5 20
4 8 2 6 5
```

### Sample Output
```text
4
```

### Explanation
Sort the scholarship requirements in increasing order and award scholarships starting from the smallest amount. Students requiring 2, 4, 5, and 6 can be selected within the budget of 20.

---

### Solution (Python)
```python
import sys

def solve():
    # Fast I/O
    input_data = sys.stdin.read().split()
    if not input_data:
        return

    # Read N (number of students) and B (total budget)
    N = int(input_data[0])
    B = int(input_data[1])

    # Read scholarship requirements of all students
    scholarships = [int(x) for x in input_data[2:2 + N]]

    # Sort requirements in ascending order
    scholarships.sort()

    count = 0
    current_spent = 0

    for req in scholarships:
        if current_spent + req <= B:
            current_spent += req
            count += 1
        else:
            break

    print(count)

if __name__ == '__main__':
    solve()
```

---

## Problem 2: Maximum Learning Points (Dynamic Programming)

### Problem Description
A student is preparing for a programming contest. Every day, the student can solve one topic, and each topic provides a certain number of learning points. However, the student cannot solve two consecutive difficult topics because of fatigue.

Given the learning points of each topic arranged in order, determine the maximum learning points the student can earn without selecting two consecutive topics.

### Input Format
- **First line:** `N` (number of topics)
- **Second line:** `N` integers representing learning points.

### Output Format
- Print the maximum learning points.

### Constraints
- $1 \le N \le 100000$
- $1 \le \text{Points} \le 10000$

### Sample Input
```text
6
5 1 2 10 6 2
```

### Sample Output
```text
17
```

### Explanation
Choose topics with points 5, 10, and 2. The total learning points are $5 + 10 + 2 = 17$. This is the maximum possible without choosing two consecutive topics.

---

### Solution (Python)
```python
import sys

def solve():
    # Fast I/O
    input_data = sys.stdin.read().split()
    if not input_data:
        return

    N = int(input_data[0])
    points = [int(x) for x in input_data[1:1 + N]]

    if N == 0:
        print(0)
        return
    if N == 1:
        print(points[0])
        return

    # prev2 represents DP[i-2], prev1 represents DP[i-1]
    prev2 = 0
    prev1 = 0

    for pt in points:
        curr = max(prev1, prev2 + pt)
        prev2 = prev1
        prev1 = curr

    print(prev1)

if __name__ == '__main__':
    solve()
```
