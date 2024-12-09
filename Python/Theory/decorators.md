# Decorators
The Python language provides a simple yet powerful syntax called ‘decorators’. A decorator is a function or a class that wraps (or decorates) a function or a method. The ‘decorated’ function or method will replace the original ‘undecorated’ function or method. Because functions are first-class objects in Python, this can be done ‘manually’, but using the @decorator syntax is clearer and thus preferred.
``` python
def foo():
    # do something

def decorator(func):
    # manipulate func
    return func

foo = decorator(foo)  # Manually decorate

@decorator
def bar():
    # Do something
# bar() is decorated
```

This mechanism is useful for separating concerns and avoiding external unrelated logic ‘polluting’ the core logic of the function or method. A good example of a piece of functionality that is better handled with decoration is [memoization](https://en.wikipedia.org/wiki/Memoization#Overview) or caching: you want to store the results of an expensive function in a table and use them directly instead of recomputing them when they have already been computed. This is clearly not part of the function logic.

---

In Python, a decorator is a design pattern that allows you to modify the functionality of a function by wrapping it in another function.

The outer function is called the decorator, which takes the original function as an argument and returns a modified version of it.

```python
def make_pretty(func):

    def inner():
        print("I got decorated")
        func()
    return inner

@make_pretty
def ordinary():
    print("I am ordinary")

ordinary()  

# OUTPUT
# I got decorated
# I am ordinary

```

## Decorating Functions with Parameters
```python
def smart_divide(func):
    def inner(a, b):
        print("I am going to divide", a, "and", b)
        if b == 0:
            print("Whoops! cannot divide")
            return

        return func(a, b)
    return inner

@smart_divide
def divide(a, b):
    print(a/b)

divide(2,5)

divide(2,0)
```

## Chaining Decorators in Python

Multiple decorators can be chained in Python.

To chain decorators in Python, we can apply multiple decorators to a single function by placing them one after the other, with the most inner decorator being applied first.



---
### Example

```python
from functools import wraps

def decorator(f):
	# do stuff when functions gets decorated
	@wraps(f)
	def wrapped(*args, **kwargs):
		# do stuff before decorated function is executed
		return f(*args, **kwargs)
		# do stuff after decorated function is executed
	return wrapped
```

## Parametrized Decorator
```python
def parametrized_decorator(param):
	def decorator(f):
		@wraps(f)
		def wrapped(*args, **kwargs):
			# do stuff with param(s)
			return f(*args, **kwargs)
		# do stuff with wrapped decorated function
		return wrapped
	# do stuff with decorator
	return decorator
```

python functools - wraps decorator:
https://stackoverflow.com/questions/308999/what-does-functools-wraps-do

https://github.com/GrahamDumpleton/wrapt/blob/develop/blog/01-how-you-implemented-your-python-decorator-is-wrong.md
