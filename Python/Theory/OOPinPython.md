# Object-oriented paradigm in Python, considerations

There are some reasons to avoid unnecessary object-orientation. Defining custom classes is useful when we want to glue some state and some functionality together. The problem, as pointed out by the discussions about functional programming, comes from the “state” part of the equation.

In some architectures, typically web applications, multiple instances of Python processes are spawned as a response to external requests that happen simultaneously. In this case, holding some state in instantiated objects, which means keeping some static information about the world, is prone to concurrency problems or race conditions.
Sometimes, between the initialization of the state of an object (usually done with the `__init__()` method) and the actual use of the object state through one of its methods, the world may have changed, and the retained state may be outdated.

*This and other issues led to the idea that using stateless functions is a better programming paradigm.*

Another way to say the same thing is to suggest using functions and procedures with as few implicit contexts and side-effects as possible. A function’s implicit context is made up of any of the global variables or items in the persistence layer that are accessed from within the function. Side-effects are the changes that a function makes to its implicit context. If a function saves or deletes data in a global variable or in the persistence layer, it is said to have a side-effect.

Carefully isolating functions with context and side-effects from functions with logic (called pure functions) allows the following benefits:

- Pure functions are deterministic: given a fixed input, the output will always be the same.
- Pure functions are much easier to change or replace if they need to be refactored or optimized.
- Pure functions are easier to test with unit tests: There is less need for complex context setup and data cleaning afterwards.
- Pure functions are easier to manipulate, decorate, and pass around.

In summary, pure functions are more efficient building blocks than classes and objects for some architectures because they have no context or side-effects.

