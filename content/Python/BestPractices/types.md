# Typing
Python is a dinamically typed language.

The dynamic typing of Python is often considered to be a weakness, and indeed it can lead to complexities and hard-to-debug code. Something named ‘a’ can be set to many different things, and the developer or the maintainer needs to track this name in the code to make sure it has not been set to a completely unrelated object.

Some guidelines help to avoid this issue:

- Avoid using the same variable name for different things.

**Bad**
```python
a = 1
a = 'a string'
def a():
    pass  # Do something
```
**Good**
```python
count = 1
msg = 'a string'
def func():
    pass  # Do something
```
Using short functions or methods helps to reduce the risk of using the same name for two unrelated things.

It is better to use different names even for things that are related, when they have a different type:

**Bad**
```python
items = 'a b c d'  # This is a string...
items = items.split(' ')  # ...becoming a list
items = set(items)  # ...and then a set
```
There is no efficiency gain when reusing names: the assignments will have to create new objects anyway. However, when the complexity grows and each assignment is separated by other lines of code, including ‘if’ branches and loops, it becomes harder to ascertain what a given variable’s type is.


### Mutable and immutable types
Python has two kinds of built-in or user-defined types.

Mutable types are those that allow in-place modification of the content. Typical mutables are lists and dictionaries: All lists have mutating methods, like `list.append()` or `list.pop()`, and can be modified in place. The same goes for dictionaries.
Immutable types provide no method for changing their content. For instance, the variable x set to the integer 6 has no “increment” method. If you want to compute x + 1, you have to create another integer and give it a name.

One consequence of this difference in behavior is that mutable types are not “stable”, and therefore cannot be used as dictionary keys.

Using properly mutable types for things that are mutable in nature and immutable types for things that are fixed in nature helps to clarify the intent of the code.

For example, the immutable equivalent of a list is the tuple, created with `(1, 2)`. This tuple is a pair that cannot be changed in-place, and can be used as a key for a dictionary.

One peculiarity of Python that can surprise beginners is that strings are immutable. This means that when constructing a string from its parts, appending each part to the string is inefficient because the entirety of the string is copied on each append. Instead, it is much more efficient to accumulate the parts in a list, which is mutable, and then glue (`join`) the parts together when the full string is needed. List comprehensions are usually the fastest and most idiomatic way to do this.

**Bad**
```python
# create a concatenated string from 0 to 19 (e.g. "012..1819")
nums = ""
for n in range(20):
    nums += str(n)   # slow and inefficient
print(nums)
```
**Better**
```python
# create a concatenated string from 0 to 19 (e.g. "012..1819")
nums = []
for n in range(20):
    nums.append(str(n))
print("".join(nums))  # much more efficient
```
**Best**
```python
# create a concatenated string from 0 to 19 (e.g. "012..1819")
nums = [str(n) for n in range(20)]
print("".join(nums))
```

One final thing to mention about strings is that using `join()` is not always best. In the instances where you are creating a new string from a pre-determined number of strings, using the addition operator is actually faster. But in cases like above or in cases where you are adding to an existing string, using `join()` should be your preferred method.
```python
foo = 'foo'
bar = 'bar'

foobar = foo + bar  # This is good
foo += 'ooo'  # This is bad, instead you should do:
foo = ''.join([foo, 'ooo'])
```
Note
	You can also use the [%](https://docs.python.org/3/library/string.html#string-formatting "(in Python v3.11)") formatting operator to concatenate a pre-determined number of strings besides [`str.join()`](https://docs.python.org/3/library/stdtypes.html#str.join "(in Python v3.11)") and `+`. However, [**PEP 3101**](https://www.python.org/dev/peps/pep-3101) discourages the usage of the `%` operator in favor of the [`str.format()`](https://docs.python.org/3/library/stdtypes.html#str.format "(in Python v3.11)") method.
	
```python
foo = 'foo'
bar = 'bar'

foobar = '%s%s' % (foo, bar) # It is OK
foobar = '{0}{1}'.format(foo, bar) # It is better
foobar = '{foo}{bar}'.format(foo=foo, bar=bar) # It is best
```

&nbsp;
# import modules only for type checking
```python
from typing import TYPE_CHECKING
if TYPE_CHECKING:
    from main import Main
```