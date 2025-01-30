# Instance, Class and Static Methods

```python
class MyClass:
    def method(self):
        return 'instance method called', self

    @classmethod
    def classmethod(cls):
        return 'class method called', cls

    @staticmethod
    def staticmethod():
        return 'static method called'
```


### Instance Methods

The first method on `MyClass`, called `method`, is a regular _instance method_. That’s the basic, no-frills method type you’ll use most of the time. You can see the method takes one parameter, `self`, which points to an instance of `MyClass` when the method is called (but of course instance methods can accept more than just one parameter).

Through the `self` parameter, instance methods can freely access attributes and other methods on the same object. This gives them a lot of power when it comes to modifying an object’s state.

Not only can they modify object state, instance methods can also access the class itself through the `self.__class__` attribute. This means instance methods can also modify class state.

### Class Methods
Let’s compare that to the second method, `MyClass.classmethod`. I marked this method with a [`@classmethod`](https://docs.python.org/3/library/functions.html#classmethod) decorator to flag it as a _class method_.

Instead of accepting a `self` parameter, class methods take a `cls` parameter that points to the class—and not the object instance—when the method is called.

Because the class method only has access to this `cls` argument, it can’t modify object instance state. That would require access to `self`. However, class methods can still modify class state that applies across all instances of the class.

### Static Methods
The third method, `MyClass.staticmethod` was marked with a [`@staticmethod`](https://docs.python.org/3/library/functions.html#staticmethod) decorator to flag it as a _static method_.

This type of method takes neither a `self` nor a `cls` parameter (but of course it’s free to accept an arbitrary number of other parameters).

Therefore a static method can neither modify object state nor class state. Static methods are restricted in what data they can access - and they’re primarily a way to [namespace](https://realpython.com/python-namespaces-scope/) your methods.

---
A static method in Java does not translate to a Python classmethod. It results in more or less the same effect, but the goal of a classmethod is actually to do something that’s usually not even possible in Java (like inheriting a non-default constructor). The idiomatic translation of a Java static method is usually a module-level function, not a classmethod or staticmethod. (And static final fields should translate to module-level constants.)

## The difference between the Class method and the static method is:
- A class method takes cls as the first parameter while a static method needs no specific parameters.
- A class method can access or modify the class state while a static method can’t access or modify it.
- In general, static methods know nothing about the class state. They are utility-type methods that take some parameters and work upon those parameters. On the other hand class methods must have class as a parameter.
- We use @classmethod decorator in python to create a class method and we use @staticmethod decorator to create a static method in python.

The class method is also known as *bound method* or *instance method*, while the staticmethod is known as *unbound method*.

## **When to use the class or static method?**

- We generally use the class method to create factory methods. Factory methods return class objects ( similar to a constructor ) for different use cases.
- We generally use static methods to create utility functions.

### Notes
- All those Foo.Bar.Baz attribute chains don’t come for free, either. In Java, those dotted names are looked up by the compiler, so at runtime it really doesn’t matter how many of them you have. In Python, the lookups occur at runtime, so each dot counts. (Remember that in Python, “Flat is better than nested”, although it’s more related to “Readability counts” and “Simple is better than complex,” than to being about performance.)
