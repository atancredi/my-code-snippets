##### Generated with Gemini 2.5 Pro (experimental)

Decorating a class is very similar syntactically to decorating a function, but the decorator works on the class object itself rather than a function object.

The Basic Idea

Define a Decorator Function: This function will accept one argument, which will be the class being decorated.
Modify the Class: Inside the decorator function, you can add attributes, add methods, wrap existing methods, or even replace the class entirely with a new one (though modifying is more common).
Return the Class: The decorator function must return a class object – either the modified original class or a new class.
Apply the Decorator: Use the @decorator_name syntax directly above the class definition.
How it Works Under the Hood

When you write:

```Python
@my_class_decorator
class MyClass:
    # ... class definition ...
    pass
```
Python translates this into:

```Python
class MyClass:
    # ... class definition ...
    pass

MyClass = my_class_decorator(MyClass) # The decorator is called with the class object
```

So, the decorator function receives the class MyClass as its argument (cls in the examples below) and the return value of the decorator function is then assigned back to the name MyClass.

Examples

1. Adding a Class Attribute

This is a simple way to attach extra data or metadata to a class.

```Python
def add_version(version_number):
  """Decorator factory to add a __version__ attribute to a class."""
  def decorator(cls):
    print(f"--- Decorating {cls.__name__} with version {version_number} ---")
    cls.__version__ = version_number
    return cls # Return the modified class
  return decorator

# Using the decorator factory
@add_version("1.0.1")
class MyDataHandler:
  def __init__(self, data):
    self.data = data
  def process(self):
    print(f"Processing data: {self.data}")

# Now the class has the __version__ attribute
print(f"MyDataHandler version: {MyDataHandler.__version__}")

handler = MyDataHandler("Sample Data")
handler.process()
```

Explanation:

We used a decorator factory (add_version) here, which allows us to pass arguments to the decorator ("1.0.1"). The factory returns the actual decorator function (decorator).
The decorator function takes the class (cls) as input.
It adds cls.__version__ = version_number.
Crucially, it returns cls.
2. Adding a Method

You can inject new methods into the decorated class.

```Python
def add_greeting_method(cls):
  """Decorator to add a 'greet' method to a class."""
  print(f"--- Adding 'greet' method to {cls.__name__} ---")

  def greet(self): # 'self' will be the instance when called
      print(f"Hello from an instance of {cls.__name__}!")

  cls.greet = greet # Assign the function as a method to the class
  return cls # Return the modified class

@add_greeting_method
class MyWidget:
  def __init__(self, name):
    self.name = name

# Now instances of MyWidget have the greet method
widget = MyWidget("Cool Button")
widget.greet()
print(f"Widget name: {widget.name}")
```

Explanation:

The add_greeting_method decorator receives the MyWidget class.
It defines a new function greet. Note that greet takes self as its first argument, just like a regular instance method.
It assigns this greet function to the class as an attribute (cls.greet = greet). When called on an instance, Python automatically handles passing the instance as self.
It returns the modified cls.
3. Modifying Class Behavior (e.g., Logging Instantiation)

This is a more advanced example where we wrap the class's __init__ method.

```Python

import functools

def log_instantiation(cls):
  """Decorator to log when instances of a class are created."""
  print(f"--- Setting up instantiation logging for {cls.__name__} ---")
  original_init = cls.__init__ # Store the original __init__

  # Define a wrapper for __init__
  @functools.wraps(original_init) # Preserves original __init__ metadata (like docstring)
  def new_init(self, *args, **kwargs):
    print(f"INFO: Creating an instance of {cls.__name__} with args={args}, kwargs={kwargs}")
    original_init(self, *args, **kwargs) # Call the original __init__

  # Replace the class's __init__ with the wrapper
  cls.__init__ = new_init
  return cls # Return the modified class

@log_instantiation
class DatabaseConnection:
  def __init__(self, db_url, timeout=30):
    """Initializes the database connection."""
    self.url = db_url
    self.timeout = timeout
    print(f"  (Inside original __init__ of DatabaseConnection: Connecting to {self.url})")

  def fetch_data(self):
    print(f"Fetching data using connection {self.url}...")

# Creating instances will now trigger the log message
print("\nCreating first connection:")
conn1 = DatabaseConnection("postgresql://user:pass@host:port/db", timeout=60)
conn1.fetch_data()

print("\nCreating second connection:")
conn2 = DatabaseConnection("mysql://user@host/db") # Using default timeout
conn2.fetch_data()
```

Explanation:

The decorator saves the original __init__ method.
It defines a new_init function that first prints a log message and then calls the original_init with all the arguments it received (*args, **kwargs).
functools.wraps(original_init) is used to copy metadata (like the docstring and name) from the original __init__ to new_init. This is good practice for decorators that wrap functions or methods.
The class's __init__ is replaced with new_init.
The modified class is returned.
Common Use Cases for Class Decorators:

Registration: Automatically registering classes in a central registry (e.g., for plugins, serializers, admin interfaces).
Adding Functionality: Injecting common methods or attributes (like logging, versioning, debugging tools).
Enforcing Patterns: Implementing patterns like Singleton (though often debated if decorators are the best way).
Instrumentation/Monitoring: Adding code to track instantiation, method calls, etc.
Transparently Modifying Behavior: Wrapping methods like __init__ or others.
Remember the key is that a class decorator receives the class object itself and must return a class object (usually the modified input class).