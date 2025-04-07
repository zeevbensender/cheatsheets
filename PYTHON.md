# Python

## Collections
### Generate a dictionary from two lists of equal length using zip() and dictionary comprehension

```python
# ✅ Using zip() with dict()
keys = ["a", "b", "c"]
values = [1, 2, 3]

result_dict = dict(zip(keys, values))
print(result_dict)  # Output: {'a': 1, 'b': 2, 'c': 3}

# ✅ Using Dictionary Comprehension
python
Copy
result_dict = {key: value for key, value in zip(keys, values)}
print(result_dict)  # Output: {'a': 1, 'b': 2, 'c': 3}
```

### Extract the name attribute from a list of dictionaries using list comprehension
```python
# ✅ Using List Comprehension
data = [
    {"name": "Alice", "age": 25},
    {"name": "Bob", "age": 30},
    {"name": "Charlie", "age": 35}
]

names = [item["name"] for item in data]
print(names)  # Output: ['Alice', 'Bob', 'Charlie']
```
