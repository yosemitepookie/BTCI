# Debugging

## Common Python Bugs 
1. Shared inner list: This creates rows which all reference the same list.
    ```python
    # This is the same as: 
    grid = [[0] * cols] * rows

    # This: 
    row = [0, 0, 0]
    grid = [row] * 3
    ```

    - This means that if you do `grid[0][1] = 10`, then `grid[1][1]`, will also return 10. 
    - The correct way to do it is: 
    ```python
    grid = [[0] * cols for _ in range(rows)]
    ```
        - Here python is executing [0] * cols, X seperate times!!!

    - How to explain it: 
        - This is an issue since all of the rows are referencing the same list since Python lists stores references to objects, not copies of objects. 
        - So mutating one row, mutates all rows. 
        - I would replace it with a list comprehension so that each row is independetly allocated. 
    - It can be proved with: 
        ```python
        print(grid[0] is grid[1]) # This will print True

        print(id(grid[0])) 
        print(id(grid[1]))
        print(id(grid[2]))
        # These will all print the same memory address, which is how we know it is the same object. 
        ```
2. Mutable Default Argument: Python evaluates default arguments once at function definition time. So all calls that omit the default argument would share the same list, dict, or set. 
    ```python 
    def add_item(x, items=[]): 
        items.append(x)
        return items 
    ```
    - This should instead be: 
    ```python 
    def add_item(x, items): 
        if items is None: 
            items = []
        items.append(x)
        return items
    ```
    - Avoid mutable default arguments like `[]`, `{}`, and `set()` unless you want shared state across calls. 
3. Class variable shared across instances. 
    - Here data is a class attribute, not an instance attribute. So this means it will be shared by every instance of `Cache`. 
    ```python 
    class Cache: 
        data = {}

    # SO instead you would want to do 

    class Cache: 
        def __init__(self): 
            self.data = {}
    ```
    - To explain: data is defined for the class so it is shared across instances. And since dictionaries are mutable, mutating through one instance would affect every instance. 
    - It would make sense to use class attributes for shared constants or shared configs. 
    ```python 
    class Cache:
        max_size = 100
    ```
4. String timestamp comparison instead of datetime comparison. 
    - When we compare strings, we compare it character by character. 
        ```python 
        # Technically here the first date is seen as greater. 
        "2026-2-05" > "2026-10-15" 
        ```
    - So comparing strings for dates is only reliable when each component has a fixed width: `YYYY-MM-DD`
    - Instead of comparing strings, we should parse strings into datetime objects. 
    ```python 
    from datetime import datetime 

    # Strptime, is string parse time. 
    d2 = datetime.strptime("2026-10-01", "%Y-%m-%d").date()

    events.sort(
        key=lambda e: datetime.strptime(e["timestamp"], "%Y-%m-%d")
    )
    ```
5. Thruthiness bug. 
        ```python 
        if value: 
            ... 
        
        # Is the same as 

        if value is True: 

        ```
    - But a lot of things are considered False in Python. And so in these cases, the code would skip over them. 
        - 0, 0.0, {}, [], set(), None, False 
        - The bug is that the above could be considered valid inputs, but this condition would treat them as invalid. 
    -  So you should choose the condition for the if based on intent: 
        ```python
        # If value is missing 
        if value is not None: 
            ...

        # If value is list, set, or dictionary, and is empty
        if len(value) > 0: 
            ... 
        
        if value is True: 

        ```
    - To say in an interview: the bug is the condition relies on truthiness. I would replace this check with a more explicit condition. 
6. `is` vs `==`
    - `==` checks that two values are equal. So if two things have the same contents, this will be true. 
    - `is` checks that the two variables point to the same object in memory. 
        ```python 
        a = 1000
        b = 1000

        print(a == b)  # True
        print(a is b)  # may be False. This is because python has some caching that it might reuse internally. DO NOT depend on this. 

        if word == "good': 
            XXX

        if count == 5: 
            XXX
        ```
    - BUT!!!! You can do `is` for singleton objects. 
        - Singleton objects are objects that there are only one instance of it in the program
        - Examples are None, True
7. In Place Mutation: This means you are changing the contents of an existing item without creating a new one. 
    - So these in place mutations are going to be in lists, dictionaries, and sets. These are the mutables. 
        - Lists: `append()`, `extend()`, `insert()`, `remove()`, `pop()`, `sort()`, `reverse()`
        - Dictionaries: `update()`, `pop()`, `clear()`
        - Sets: `add()`, `remove()`, `discard()`, `update()`
    - Since they all mutate the object they return None 
    - If you don't want to mutate the object, and instead create a new one, you would do: 
        - `sorted(list)`, `list(reverse(list))`
            - reverse is just an iterator obj, and so you have to make it into a list. 
        - These return the new list. 
        - For append: `new_nums = nums + [4]` 
    - Mutables on the other hand are things like ints, floats, bool, tuple, strings. 
        - Although it looks like we might be changing the value, Python is actually creating a whole new item. 
    - If you want to avoid mutation, then you should make a copy first. 
        - This is valuable where you are sending a list as an argument to a function, and then inside the function modifying the list. 
            - BUT you may not want the list to be modified outside of the list 
    - This can come up while you iterate through a list and try to delete at the same time. 
        ```python 
        nums = [1, 2, 4, 5]

        for x in nums: 
            if x % 2 == 0: 
                nums.remove(x)
        print(nums) # outputs [1, 4, 5]

        # 4 was skipped, because after removing 2, the list shifted. 

        nums = [x for x in nums if x%2 == 0] # Creates a new list 

        for x in nums[:]: # Iterates over a copy while mutating the original 
            if x%2 == 0: 
                nums.remove(x)
        ```
    - If you don't want to mutate the existing list or dictionary that was passed in: 
        ```python 
        do list/dict = list/dict.copy()
        ```
    - You use `copy.deepcopy(grid/etc)` when you have something with nested mutable objects. 
        - This would be like a grid. 
        - Or a list of dictionaries, a dict of dicts
            ```python 
            import copy 

            new_grid = copy.deepcopy(grid)
            ```
    - How to go about saying this: 
        - A shallow copy creates a new outer container, but this means that the nested mutable objects are still shared. 
        - A deep copy recursively copies the nested objects too. 
        - I would use shallow copy for flat lists and dictionaries, and a deep copt when mutating nested lists, dictionaries, or custom objects. 
8. Off-by-one loop bounds 
    - Happens when you are comparing adjacent elements: 
        ```python 
        for i in range(len(nums)): 
            if nums[i] > nums[i+1]: 
                return False 
        
        # This should be 
        for i in range(len(nums) - 1): 
            if nums[i] > nums[i+1]: 
                return False 
        ```
    - Another common error is binary search bounds: 
        ```python 
        # Closed interval version 
        left = 0 
        right = len(nums) - 1

        while left <= right: 
            mid = (left + right) // 2 
            if nums[mid] < target: 
                left = mid + 1 
            elif nums[mid] > target: 
                right = mid - 1 
            else: 
                return mid 

        # Open interval version 
        left = 0
        right = len(nums)

        while left < right:
            mid = (left + right) // 2

            if nums[mid] < target:
                left = mid + 1
            elif nums[mid] > target:
                right = mid
            else:
                return mid

        ```
9. Wrong Exception Handling
    - You don't want to have generic exeception handling, because then it could hide a real bug, ctach too much, or handle the error in a way that changes the program's meaning 
        ```python 
        # Don't do 
        try: 
            value = int(user_input)
        except: 
            value = 0 

        ```
    - A common interview debugging is catching the wrong type of exception. 
        - KeyError: missing keys in dictionaries
        - IndexError: missing indexes in lists 
        - ValueError 
        - TypeError: 
        - ZeroDivisionError: 
10. Shared State across tests: 
    - Why does this test pass alone, but fail when the whole suite runs?
        - Strong signal for shared state, test ordering, caching, global mutation
    - Each test should be able to run alone or in any order. 
    - A clean test should: 
        - Create a fresh state
        - Run the behavior 
        - Check the result 
        - Restore anything external
11. Delete Keys from Dict while Iterating 
    ```python 
    for key in d: 
        if key % 2 == 0: 
            del d[key]
    
    # This will return a runtime error since the dictionary changed size during iteration!!

    # Instead!!!! Iterate over a copy of the keys 

    for key in list(d.keys()): 
        if key % 2 == 0: 
            del d[key]
    ```


Boopers do: how to delete from a dictionary while iterating through it 
How to find the duplicate in a list?? 