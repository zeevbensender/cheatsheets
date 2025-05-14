# Python

## File System
### ✅ Create a directory if it doesn't exist
```python
from pathlib import Path
Path("/my/directory").mkdir(parents=True, exist_ok=True)
```

### ✅ Delete a directory if it exists
```python
import shutil
shutil.rmtree('/path/to/your/dir/')
```

## Collections
### Generate a dictionary from two lists of equal length using zip() and dictionary comprehension


#### ✅ Using zip() with dict()
```python
keys = ["a", "b", "c"]
values = [1, 2, 3]

result_dict = dict(zip(keys, values))
print(result_dict)  # Output: {'a': 1, 'b': 2, 'c': 3}
```
#### ✅ Using Dictionary Comprehension
```python
result_dict = {key: value for key, value in zip(keys, values)}
print(result_dict)  # Output: {'a': 1, 'b': 2, 'c': 3}
```

### Extract the name attribute from a list of dictionaries using list comprehension
#### ✅ Using List Comprehension
```python
data = [
    {"name": "Alice", "age": 25},
    {"name": "Bob", "age": 30},
    {"name": "Charlie", "age": 35}
]

names = [item["name"] for item in data]
print(names)  # Output: ['Alice', 'Bob', 'Charlie']
```

### Create a new dictionary from another dictionary by filtering entries whose values meet a condition using dictionary comprehension.

#### ✅ Dictionary Comprehension

```python
filtered_dict = {key: value for key, value in original_dict.items() if value.visible}

# Creates a new dictionary
# Includes only entries where value.visible == True

class Item:
    def __init__(self, name, visible):
        self.name = name
        self.visible = visible

original_dict = {
    "a": Item("Item A", True),
    "b": Item("Item B", False),
    "c": Item("Item C", True),
}

filtered_dict = {key: value for key, value in original_dict.items() if value.visible}

# Printing filtered results
print(filtered_dict)  # Output: {'a': <Item object>, 'c': <Item object>}
```
✅ Only entries where value.visible == True remain in filtered_dict.



#### ✅ Alternative: Using filter()
```python
filtered_dict = dict(filter(lambda item: item[1].visible, original_dict.items()))
```
✅ More functional-style but less readable than dictionary comprehension.

### Pretty print a dictionary
```python
import json

print(json.dumps(
    {'4': 5, '6': 7},
    sort_keys=True,
    indent=4,
    separators=(',', ': ')
))
```
