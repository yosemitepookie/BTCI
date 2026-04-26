# Wanna Revisit: 
- Binary Search: Guess and Check 

### Floor Division 

```python 
x = x // 10 

```


### Sorting by the first element in the list 
```python 
intervals.sort(key=lambda x: x[0])
```


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