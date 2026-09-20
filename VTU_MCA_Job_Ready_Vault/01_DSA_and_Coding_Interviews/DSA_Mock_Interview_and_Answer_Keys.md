# DSA Mock Coding Interviews & Official Answer Keys
**Format**: Realistic 45-minute technical coding interview simulation with Interviewer prompts, optimal code, and edge case discussion.

---

## 🎯 Mock Interview 1: Two Sum with Duplicates & Indices
* **Difficulty**: Medium
* **Interviewer Prompt**:
  > *"Given an array of integers `nums` and an integer `target`, return all unique pairs `[a, b]` such that `a + b == target`. Each element in `nums` may only be used once in each pair, and the output must not contain duplicate pairs. After writing this, explain how you would scale this if the array has 10 billion numbers stored across a distributed cluster."*

### 💡 Optimal Answer Key & Code
```python
def two_sum_all_pairs(nums: list[int], target: int) -> list[list[int]]:
    nums.sort()
    left = 0
    right = len(nums) - 1
    result = []
    
    while left < right:
        curr_sum = nums[left] + nums[right]
        if curr_sum == target:
            result.append([nums[left], nums[right]])
            left += 1
            right -= 1
            # Skip duplicates to ensure unique pairs
            while left < right and nums[left] == nums[left - 1]:
                left += 1
            while left < right and nums[right] == nums[right + 1]:
                right -= 1
        elif curr_sum < target:
            left += 1
        else:
            right -= 1
            
    return result
```
* **Time Complexity**: $O(N \log N)$ for sorting + $O(N)$ two pointers = $O(N \log N)$.
* **Space Complexity**: $O(1)$ extra memory beyond sorting stack.
* **Distributed Follow-up Answer**:
  > *"For 10 billion numbers, data exceeds single-machine RAM. We partition the data using MapReduce / Apache Spark by assigning each number $x$ to partition $P = \text{hash}(\min(x, \text{target} - x))$. This guarantees that any two complementary numbers will end up on the exact same worker node for local hash-joining."*

---

## 🎯 Mock Interview 2: LRU (Least Recently Used) Cache
* **Difficulty**: Hard
* **Interviewer Prompt**:
  > *"Design a data structure that follows the constraints of a Least Recently Used (LRU) cache. Implement `get(key)` and `put(key, value)` both running in strictly $O(1)$ average time complexity."*

### 💡 Optimal Answer Key & Code
```python
class Node:
    def __init__(self, key=0, val=0):
        self.key = key
        self.val = val
        self.prev = None
        self.next = None

class LRUCache:
    def __init__(self, capacity: int):
        self.cap = capacity
        self.cache = {} # maps key -> Node
        # Dummy head and tail
        self.head = Node()
        self.tail = Node()
        self.head.next = self.tail
        self.tail.prev = self.head

    def _remove(self, node: Node):
        p = node.prev
        n = node.next
        p.next = n
        n.prev = p

    def _add_to_front(self, node: Node):
        node.next = self.head.next
        node.prev = self.head
        self.head.next.prev = node
        self.head.next = node

    def get(self, key: int) -> int:
        if key in self.cache:
            node = self.cache[key]
            self._remove(node)
            self._add_to_front(node) # Mark as recently used
            return node.val
        return -1

    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            self._remove(self.cache[key])
        node = Node(key, value)
        self._add_to_front(node)
        self.cache[key] = node
        
        if len(self.cache) > self.cap:
            # Evict least recently used (node before tail)
            lru = self.tail.prev
            self._remove(lru)
            del self.cache[lru.key]
```
* **Time Complexity**: $O(1)$ for both `get` and `put`.
* **Space Complexity**: $O(\text{Capacity})$ in hash map and doubly linked list.

---

## 🎯 Mock Interview 3: Trapping Rain Water
* **Difficulty**: Hard
* **Interviewer Prompt**:
  > *"Given `n` non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining. Solve it with $O(1)$ auxiliary space."*

### 💡 Optimal Answer Key & Code
```python
def trap(height: list[int]) -> int:
    if not height:
        return 0
    
    left = 0
    right = len(height) - 1
    left_max = 0
    right_max = 0
    trapped_water = 0
    
    while left < right:
        if height[left] < height[right]:
            if height[left] >= left_max:
                left_max = height[left]
            else:
                trapped_water += left_max - height[left]
            left += 1
        else:
            if height[right] >= right_max:
                right_max = height[right]
            else:
                trapped_water += right_max - height[right]
            right -= 1
            
    return trapped_water
```
* **Time Complexity**: $O(N)$ single pass.
* **Space Complexity**: $O(1)$ strictly constant extra space.

---

## 🎯 Mock Interview 4: Word Search II (Trie + Backtracking)
* **Difficulty**: Hard
* **Interviewer Prompt**:
  > *"Given an $m \times n$ board of characters and a list of strings `words`, return all words on the board. Each word must be constructed from letters of sequentially adjacent cells."*

### 💡 Optimal Answer Key
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.word = None

def find_words(board: list[list[str]], words: list[str]) -> list[str]:
    # 1. Build Trie
    root = TrieNode()
    for w in words:
        curr = root
        for ch in w:
            if ch not in curr.children:
                curr.children[ch] = TrieNode()
            curr = curr.children[ch]
        curr.word = w
        
    rows, cols = len(board), len(board[0])
    result = []
    
    def dfs(r: int, c: int, parent_node: TrieNode):
        char = board[r][c]
        if char not in parent_node.children:
            return
        
        curr_node = parent_node.children[char]
        if curr_node.word:
            result.append(curr_node.word)
            curr_node.word = None # Prevent duplicates
            
        board[r][c] = '#' # Mark visited
        for dr, dc in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
            nr, nc = r + dr, c + dc
            if 0 <= nr < rows and 0 <= nc < cols and board[nr][nc] != '#':
                dfs(nr, nc, curr_node)
        board[r][c] = char # Backtrack
        
        # Pruning optimization
        if not curr_node.children:
            del parent_node.children[char]

    for r in range(rows):
        for c in range(cols):
            dfs(r, c, root)
            
    return result
```
