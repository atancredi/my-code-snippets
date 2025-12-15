# Not using get() to return a default value from a dict

**there is a clear difference between using square brackets and get():**

- `dictionary.get('key')` returns the value associated with the specified key **if it exists in the dictionary**, or `None` if the key does not exist. This means that `dictionary.get('key')` will not throw an error if the key does not exist.
- `dictionary['key']` returns the value associated with the specified key if it exists but raises a `KeyError` if the key does not exist. Thus, `dictionary['key']` might crash your program when accessing nonexistent keys.

**To put it short, `dictionary.get('key')` is the safer way to get value from a dictionary**. You shouldn’t use `dictionary['key']` because it throws errors and doesn’t handle nonexistent keys.

---

Frequently you will see code create a variable, assign a default value to the variable, and then check a dict for a certain key. If the key exists, then the value of the key is copied into the value for the variable. While there is nothing wrong this, it is more concise to use the built-in method `dict.get(key[, default])` from the Python Standard Library. If the key exists in the dict, then the value for that key is returned. If it does not exist, then the default value specified as the second argument to `get()` is returned. Note that the default value defaults to `None` if a second argument is not provided.

## Anti Pattern
```python
dictionary = {"message": "Hello, World!"}

data = ""

if "message" in dictionary:
    data = dictionary["message"]

print(data)  # Hello, World!
```

Although there is nothing wrong with this code, it is verbose and inefficient because it queries the dictionary twice. The solution below demonstrates how to express the same idea in a more concise manner by using `dict.get(key[, default])`.

## Best Practice
```python
dictionary = {"message": "Hello, World!"}

data = dictionary.get("message", "")

print(data)  # Hello, World!
```

When `get()` is called, Python checks if the specified key exists in the dict. If it does, then `get()` returns the value of that key. If the key does not exist, then `get()` returns the value specified in the second argument to `get()`.