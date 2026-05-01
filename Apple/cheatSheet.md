# Wanna Revisit: 
- Binary Search: Guess and Check 
- Recursion Chapter 33 .....
- Linked List: Reversing a linked list (for single and double)
- Trees: how does the diameter of a tree work? also just combining stuff in general 
- Heaps: practice two heaps for median tracking
- Sliding window: k counting problems

### Floor Division 

```python 
x = x // 10 

math.inf

```


# Boopp!!!!!!!

## Chapter 27 - Two Pointers 
### Triggers: 
- Input: One or two arrays, strings, linked lists. 
- Requirement: Use O(1) extra memeory or modify input in place. 
- Keywords: Palindrome, reverse, swap, merge, parition


**Types of Pointer Problems:** 
    - Inward pointers (moving into an array)
    - Slow and fast pointers 
    - Parallel pointer

### Examples: 
Palindromic Sentence:
```python 
def palindromic_sentence(s): 
    l, r = 0, len(s) - 1 
    while l < r: 
        if not s[l].isalpha():
            l += 1 
        elif not s[r].isalpha(): 
            r -= 1 
        else: 
            if s[l].lower() != s[r].lower(): 
                return False 
            l += 1 
            r -= 1 
        
    return True 
```

Seeker and Writer Pointers: 
- The Seeker pointer scans the input data 
- The Writer pointer decides where to place the next valid result. 
- Left side is the clean/processed side

```python 
def seeker_writer_recipe(arr): 
    seeker, writer = 0, 0 
    if we need to keep arr[seeker]: 
        arr[writer] = arr[seeker]
        advice writer and seeker 
    else: 
        only advance seeker 
```


## Chapter 28 - Grids and Matrices 
- Have an `is_valid()` method. 

```python 
def is_valid(r, c): 
    return 0<=r<ROWS and 0<=c<COLS

directions = [[1, 0], [0, 1], [-1, 0], [0, -1]]
for dr, dc in directions: 
    do something 
```

Intializing an empty grid: 
```python 
grid = [[0]*num_cols for _ in range(num_rows)]
```

Copy an existing grid: 
```python 
grid_copy = [row.copy() for row in grid]
```

### Grid DFS and BFS 
```python 
def grid_dfs(grid, start_r, start_c): 
    directions = [[1, 0], [-1, 0], [0, 1], [0, -1]]
    visited = set()

    def is_valid(r, c): 
        return 0 <= r < len(grid) and 0 <= c < len(grid[0]) and (r, c) not in visited

    def visit(r, c): 
        for dr, dc in directions: 
            nr, nc = r + nr, c + nc 
            if is_valid(nr, nc): 
                visited.add(nr, nc)
                visit(nr, nc)

    visited.add(start_r, start_c)
    visit(start_r, start_c)


def grid_bfs(grid, start_r, start_c): 
    visited = set()
    directions = [[1, 0], [0, 1], [-1, 0], [0, -1]]
    q = deque()
    visited.add((start_r, start_c))
    q.append((start_r, start_c))

    def is_valid(r, c): 
        return 0 <= r < len(grid) and 0 <= c < len(grid[0]) and (r, c) not in visited

    while q: 
        r, c = q.popleft()
        for dr, dc in directions: 
            nr, nc = r+dr, c+dc 
            if is_valid(nr, nc): 
                visited.add((nr, nc))
                q.append((nr, nc))

```

## Chapter 29 - Binary Search 
- Input: sorted array or string. Or an optimization problem that is hard to optimize directly 
- Keywords: sorted, threshold, range, boundary, find, search, min/max, first/last, smallest/largest 

Every binary search solution can be reframed as finding a transition point. 
```python 
def transition_point_recipe(): 
    def is_before(val): # to return whether val is before transition point
    l, r = first and last values in range 
    handle edge cases: 
        - the range is empty 
        - l is "after" the transition point 
        - r is "before" the transition point 

    while r-l > 1: 
        mid = (l+r) //2 
        if is_before(mid): # If MID is before the transition point 
            l = mid 
        else: # MID is after the transition point 
            r = mid 
    
    # NOW l and r are next to each other
    return l (the last before), r (the first after), or something else depending on the prob

```

**Review**: Guess and Check 


## Chapter 30 - Sets and Maps 
- Think about whether you can use a set instead of a map. 
- Starting from 3.7, Python gaurentees that set/map iteration order matches insertion order. 
- You cannot modify the keys in sets and dictionaries. 
    - Which is why we can't use a variable with a mutable type (like an array). Use tuple instead. 
    - This is because if a set element changes, its hash will also change. 

Methods: 
```python 
# Dictionaty 
for key, value in my_dict.items():
    print(key, value)

keys: List = my_dict.keys()
values: List = my_dict.values()

# Delete a key/value pair from a dictionary 
del my_dict[key]

# Sets 
add(), copy(), issubset(<=), issuperset(=>), intersection(&)
my_set.remove(item) # Raises an error if the item doesn't exist 
my_set.discard(item)
my_set.pop() # Removes a random item from the set. 
```


## Chapter 31 - Sorting 
- Built in sorting 
- And using the custom comparitors 
- Both sorting functions are stable (meaning they preserve the orders of the original elements w equal keys)
- Uses Timsort, which is a hybrid of merge and insertion sort
- Time complexity: O(nlogn). Best case is O(n) if it is already sorted

- `list.sort(key=key, reverse=True|False)`: modifies the list in place
    - In place means means the exisiting object is changed 
    - Returns NONE!!!! 
    - Only works on Lists!!!!
- `new_list = sorted(iterable=list, key=key, reverse=True|False)`: creates and returns a new list
    - Works on any iterable: lists, tuples, strings, and dictionaries

Understanding creating the key: 
```python 
key=lambda x: (x[0], x[1])

```


## Chapter 32 - Stacks and Queues 
- `from collections import deque`
- Adding and removing is O(1) from both ends 
- Triggers: undo/redo actions, balanced parentheses, brackets

Functions:
- `append() / appendleft()`
- `extend() / extendleft()`
- `remove(value)`: removes the first occurance of value from deque 
- `pop() / popleft()`
- `clear()`
- `count(value)`: how many times the value is in the deque 
- `rotate(k)`: rotate deque to the right by k steps. negative k rotates to the left 
- `reverse()`
- Peek from front: `deque[0]`
- Peek from end: `deque[-1]`


## Chapter 33 - Recursion


## Chapter 34 - Linked Lists 
- They are better than arrays for we want to add an element at the start, or between two elements. 

```python 
Singly Linked List: 
class Node: 
    def __init__(self, val, next = None): 
        self.val = val 
        self.next = next

class DoubleNode: 
    def __init__(self, val, next=None, prev=None): 
        self.val = val 
        self.next = None 
        self.val = None 

``` 

```python 
# Inserting an node in a Singly Linked List 
new.next = head.next 
head.next = new 

# Reversing a Singly Linked List 
def reverse_list(head): 
    prev = None 
    curr = head 
    while curr: 
        nxt = curr.next 
        curr.next = prev 
        prev = curr 
        curr = nxt 

```

```python
# Finding a cycle in a LL 

def has_cycle(head): 
    slow = head 
    fast = head 
    while fast and fast.next: 
        fast = fast.next.next 
        slow = slow.next 
        if fast == slow: 
            return False 

            # If you want to find where the cycle is.. 
            # Start slow at head and now move them one at a time. 
            slow = head 
            while slow != fast: 
                slow = slow.next 
                fast = fast.next
            return slow.val 
    return True
```

## Chapter 35 - Trees 
- Root
- Depth starts at 0 from the root 
- Height starts at 0 from the leaf

```python 
class Node: 
    def __init__(self, val, left=None, right=None): 
        self.val = val 
        self.left = left 
        self.right = right 
```

```python 
# Lazy strategy for checking nulls 

def size(node): 
    if not node: 
        return 0 
    return size(node.right) + size(node.left) + 1 
```

Traversals: 
```python 
def preorder(node): 
    if not node: 
        return 
    print(node)
    preorder(node.left)
    preorder(node.right)

```

Passing Information through a Tree: 
```python 
def visit(node, info_passed_down): 
    if base_case or node is None: 
        return base_value # What you pass UP when no node

    new_info_down = # Update the info passed down with new info from this node 
    left_up = visit(node.left, new_info_down)
    right_up = visit(node.right, new_info_down)

    # Combine something for THIS node 
    info_up = combine(left_up, right_up, node)

    # Update the global state 
    update_global(node, left_up, right_up)

    return info_to_pass_up
```
- Information you flow down: Top to Bottom 
    - Current depth 
    - Path sum 
    - Constraints like min and max value 
    - Whether the parent is valid 
- Information you flow up: Bottom to top 
    - Height of a subtree 
    - Max path sum 
    - Whether the subtree is valid
    - Count of nodes 
- Global if it depends on comparing many nodes or combining multiple subtrees
    - Max path sum, longest path, best/max/min

## Chapter 36 - Graphs 
- Represent a graph using an adj list
- Think of time complexities using V (vertices) and E (edges)
- A graph is connected if there is a path from every node to every other node


### DFS
```python
def graph_DFS(graph, start): 
    visited = {start}
    def visit(node): 
        for nbr in graph[node]: 
            if nbr not in visited:
                visited.add(nbr)
                visit(nbr)
    visit(start)

def count_connected_components(graph): 
    count = 0 
    visited = set()

    def visit(node): 
        for nbr in graph[node]: 
            if nbr not in visited:
                visited.add(nbr)
                visit(nbr)
    
    for node in range(len(graph)): 
        if node not in visited: 
            visited.add(node)
            visit(node)
            count += 1 

    return count
```

#### Path Reconstruction: 
- In DFS, when we mark a node as visited, we can also track their predecessor in the DFS. 
- Every chain of predecessors will lead to the starting node.
- We can use predecessors to keep track of visited

```python 
def path(graph, node1, node2): 
    predecessors = {node1: None} # The starting node does not have a predecessor 

    def visit(node): 
        for nbr in graph[node]: 
            if nbr not in predecessors: 
                predecessors[nbr] = node 
                visit(nbr)
    
    visit(node1)

    # There is no path
    if node2 not in predecessors: 
        return []

    path = []
    curr = node2 

    while curr is not None: 
        path.append(curr)
        curr = predecessors[curr]
    
    path.reverse()

    return path
```

### BFS 
- For connectivity (like DFS) and distance questions 
```python 
# NOT for weighted graphs. Only for UNWEIGHTED. 
def graph_BFS(graph, start): 
    q = deque()
    q.append(start)
    distances = {start: 0}
    while q: 
        node = q.popleft()
        for nbr in graph[node]: 
            if nbr not in distances: 
                distances[nbr] = distances[node] + 1 
                q.append(nbr)

```

```python 
# Multisource BFS - the same as graph BFS but add all the start nodes the queue 

def multisourceBFS(graph, sources): 
    q = deque()
    distances = {}

    for start in sources: 
        q.append(start)
        distances[start] = 0 
    
    while q:
        node = q.popleft()
        for nbr in graph[node]: 
            if nbr not in distances: 
                distances[nbr] = distances[node] + 1 
                q.append(nbr)
```



## Chapter 37 - Heaps
- Heaps are min-heaps
- Ideal for managing sorted data dynamically without fully sorting after each operation
- Used for: 
    - Heapsort for sorting 
    - Dijkstra's algorithm for shortest path in graph 
    - Prim's for minimum spanning trees 
- Restricting the heap size is a common optimization to save time and space 
- Lazy deletion 
    - Just add the element to a deleted set and then later when you pop, if it is in the deleted set don't include in final result.
- Merging m sorted lists with a heap of pointers


```python 
import heapq 

heapq.heapify(list)
heapq.heappush(list, 5)
heapq.heappop(list) # Returns the smallest elem (AKA root)
list[0] # to peek 
len(list)

# Python's heapq, the heap is ordered by the first element of whatever you push in 
# If you want to prioritize the 2nd element, just reorder it 

for name, val in data: 
    heapq.heappush(list, (val, name))

# And then just rememeber to flip again when popping it off

# ====================

heapq.nlargest(list, 2)
heapq.nsmallest(list, 2)

# =====================
# To make it a max heap!!!! 
arr = [-x for elem in list]
heapq.heapify(arr)

# To pop it off 
largest_elem = heapq.heappop(arr) * -1 
```

### When to use sorting vs heaps?
- Heaps are probably the better option if you don't need every element sorted, and just smallest/largest k. 
- Also better for dynamic data => think data structure design

### Reusable Idea: Two Heap for Median Tracking 
- Keep the lower half in a max heap, and the upper half in a min heap. 


### Merging M sorted lists with pointers
- Store data as (value, list, index in list)
```python 
def merge_lists(lists): 
    heap = []
    heapq.heapify(heap)

    for idx, list in lists:
        heapq.heappush(heap, (list[0], idx, 0))

    res = []

    while len(heap) > 0: 
        val, list_idx, idx = heapq.heappop(heap)
        res.append(val)

        idx += 1 
        if idx < len(lists[list_idx]): 
            heapq.heappush((lists[list_idx][idx], list_idx, idx))

    return res 
```


## Chapter 38 - Sliding Windows
- Consider the subarray marked by the left and right pointers
- You need the find the subarray of an input array 
- The subarray has to satisfy some contraint 
- There is an objective that makes some subarrays "better" OR sometimes have to count
- Types of problems: 
    - Fixed length windows 
    - Resetting windows: bigger window is better but a single element can make whole window invalid
    - Maximum windows
    - Minimum windows 
    - Counting problems?????



```python 
# Fixed Length Window
def fixed_length_k(arr, k): 
    l, r = 0, 0 
    # Initialize some data structure to track the current window 
    # Intialize something to track the current bestt
    
    # While we can grow the window
    while r < len(arr): 
        # Grow the window 
            # Update the data structure and increase r 
        
        # If the current window is the correct length (r-l ==k)
            # Update current best if needed 
            # Shrink window (update data structure and then increase l)
```

```python 
def resetting_window(arr): 
    l, r = 0, 0 
    # Some data structure to track window 
    # Something to track curr best 

    while r < len(arr): 
        can_grow = check if the window will still be valid with l and r
        if we can still grow: 
            grow the window: update data structure and increase r 
            update curr_best if needed 
        else: 
            reset the window past the problematic elem 
```

```python 
# Maximum window

def max_window(arr): 
    l, r = 0, 0 
    # Some dataset 
    # something to track curr best 
    while r < len(arr): 
        can_grow = 
        if can_grow: 
            # Update the ds 
            # Increase r 
            # Update curr best
        else: 
            shrink the window 
            # Update the ds and increase l 

```

```python 
# Min window

def min_window(arr): 
    l, r = 0, 0 
    # Some dataset 
    # something to track curr best 
    while True: 
        can_grow = 
        if we have to grow the window:
            if r == len(arr): # Can't grow the window 
                break 
            grow the window (update ds and increase r)
        else: 
            update curr best 
            shrink window (update ds and increase l)
    return the curr best
```

## Chapter 40 - Dynamic Programming 
- Memoization 

```python 
memo = empty map 
f(subproblem): 
    if subproblem is basecase: 
        return result 
    if subproblem in memomap: 
        return memo[subproblem]
    memo[subproblem] = recurrance relation formula 
    return memo[subproblem]

return f(initial subproblem)
```


## Chapter 42 - Topological Sort!!!! 