# DSA Mastery Roadmap & Coding Interview Patterns
**Target Audience**: VTU MCA Graduates aiming for Product MNCs, Cloud/FinTech, and Tier-1 Tech Firms.  
**Languages**: Python 3 & Java (Modern Industry Standard)

---

## 1. The 14 Core Coding Interview Patterns

Industry coding interviews do NOT test memorization—they test **Pattern Recognition**. Over 90% of LeetCode Medium/Hard interview problems reduce to one of these 14 patterns:

```
[Problem Statement]
   │
   ├─► Linear sequence with ordered pair/triplets? ─────► Pattern 1: Two Pointers
   ├─► Subarray / Substring with contiguous constraint? ─► Pattern 2: Sliding Window
   ├─► Cycle detection in linked list / arrays? ────────► Pattern 3: Fast & Slow Pointers
   ├─► Next greater/smaller element in O(N)? ───────────► Pattern 4: Monotonic Stack
   ├─► Finding K largest / smallest elements? ──────────► Pattern 5: Top K Elements (Min/Max Heap)
   ├─► Level-by-level traversal or shortest path? ──────► Pattern 6: Tree/Graph BFS
   ├─► All paths / branch exploration / permutations? ──► Pattern 7: Backtracking / DFS
   ├─► Overlapping subproblems & optimal substructure? ─► Pattern 8: Dynamic Programming
   └─► Search in sorted/rotated structure in O(log N)? ─► Pattern 9: Modified Binary Search
```

---

## Pattern 1: Two Pointers (Opposite Direction)
* **When to Use**: Sorted arrays or strings where you must find a pair meeting a target sum or condition.
* **Time Complexity**: $O(N)$ vs naive $O(N^2)$.
* **Classic Problem**: Two Sum II (Input Array Is Sorted).

### Python Implementation
```python
def two_sum_sorted(numbers: list[int], target: int) -> list[int]:
    left = 0
    right = len(numbers) - 1
    
    while left < right:
        current_sum = numbers[left] + numbers[right]
        if current_sum == target:
            return [left + 1, right + 1] # 1-indexed
        elif current_sum < target:
            left += 1   # Need larger sum
        else:
            right -= 1  # Need smaller sum
    return []
```

### Java Implementation
```java
public class TwoPointers {
    public static int[] twoSum(int[] numbers, int target) {
        int left = 0, right = numbers.length - 1;
        while (left < right) {
            int sum = numbers[left] + numbers[right];
            if (sum == target) return new int[]{left + 1, right + 1};
            if (sum < target) left++;
            else right--;
        }
        return new int[]{};
    }
}
```

---

## Pattern 2: Sliding Window (Dynamic Size)
* **When to Use**: Finding the longest/shortest contiguous subarray or substring matching a condition (e.g., unique characters, sum $\ge K$).
* **Time Complexity**: $O(N)$.
* **Classic Problem**: Longest Substring Without Repeating Characters.

### Python Implementation
```python
def length_of_longest_substring(s: str) -> int:
    char_index_map = {}
    max_len = 0
    window_start = 0
    
    for window_end in range(len(s)):
        char = s[window_end]
        if char in char_index_map and char_index_map[char] >= window_start:
            window_start = char_index_map[char] + 1
            
        char_index_map[char] = window_end
        max_len = max(max_len, window_end - window_start + 1)
        
    return max_len
```

---

## Pattern 3: Fast & Slow Pointers (Floyd's Cycle Finding)
* **When to Use**: Linked list cycle detection, finding the middle element, or detecting cyclic arrays in $O(1)$ extra space.
* **Time Complexity**: $O(N)$, Space: $O(1)$.

### Python Implementation
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def has_cycle(head: ListNode) -> bool:
    slow = head
    fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            return True
    return False

def find_middle_node(head: ListNode) -> ListNode:
    slow = head
    fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    return slow
```

---

## Pattern 4: Monotonic Stack
* **When to Use**: Next Greater Element, Daily Temperatures, Largest Rectangle in Histogram.
* **Rule**: Keep elements in strictly increasing or decreasing order. Whenever a candidate violates the order, pop and resolve until order is restored.
* **Time Complexity**: $O(N)$ because each index is pushed and popped at most once.

### Python Implementation (Daily Temperatures)
```python
def daily_temperatures(temperatures: list[int]) -> list[int]:
    n = len(temperatures)
    ans = [0] * n
    stack = []  # stores indices of unresolved temperatures
    
    for i in range(n):
        curr_temp = temperatures[i]
        while stack and temperatures[stack[-1]] < curr_temp:
            prev_day_idx = stack.pop()
            ans[prev_day_idx] = i - prev_day_idx
        stack.append(i)
        
    return ans
```

---

## Pattern 5: Top K Elements (Heap / Priority Queue)
* **When to Use**: Finding the K largest/smallest elements without sorting the full collection ($O(N \log K)$ vs $O(N \log N)$).
* **Rule**: For K largest, use a **Min-Heap** of size K. The root will hold the Kth largest element.

### Python Implementation
```python
import heapq

def find_kth_largest(nums: list[int], k: int) -> int:
    min_heap = []
    for num in nums:
        heapq.heappush(min_heap, num)
        if len(min_heap) > k:
            heapq.heappop(min_heap)
    return min_heap[0]
```

---

## Pattern 6: Tree & Graph Breadth-First Search (BFS)
* **When to Use**: Shortest path in unweighted graphs, level-order traversal, nearest neighbor.
* **Data Structure**: `collections.deque` (FIFO Queue).

### Python Implementation (Binary Tree Level Order Traversal)
```python
from collections import deque

def level_order(root) -> list[list[int]]:
    if not root:
        return []
    levels = []
    queue = deque([root])
    
    while queue:
        level_size = len(queue)
        current_level = []
        for _ in range(level_size):
            node = queue.popleft()
            current_level.append(node.val)
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        levels.append(current_level)
    return levels
```

---

## Pattern 7: Backtracking (Permutations & Subsets)
* **When to Use**: Generating all valid combinations, N-Queens, Sudoku solver.
* **Structure**: `Choose -> Explore (Recurse) -> Un-choose (Backtrack)`.

### Python Implementation (Subsets / Power Set)
```python
def subsets(nums: list[int]) -> list[list[int]]:
    result = []
    
    def backtrack(start_idx: int, current_path: list[int]):
        result.append(list(current_path))
        for i in range(start_idx, len(nums)):
            current_path.append(nums[i])       # Choose
            backtrack(i + 1, current_path)     # Explore
            current_path.pop()                 # Un-choose
            
    backtrack(0, [])
    return result
```

---

## Pattern 8: Dynamic Programming (0/1 Knapsack & 1D State)
* **When to Use**: Optimal substructure + overlapping subproblems (e.g., Longest Increasing Subsequence, Coin Change).
* **Classic Problem**: Coin Change (Fewest coins to make up an amount).

### Python Implementation
```python
def coin_change(coins: list[int], amount: int) -> int:
    # dp[i] represents min coins needed for amount i
    dp = [float('inf')] * (amount + 1)
    dp[0] = 0
    
    for a in range(1, amount + 1):
        for c in coins:
            if a - c >= 0:
                dp[a] = min(dp[a], 1 + dp[a - c])
                
    return dp[amount] if dp[amount] != float('inf') else -1
```
