I had gone over the list of things in JavaScript that causes memory leaks. They were:

1. You accidently create lots of global variables (You forget to use a const or let somewhere)
2. You create an interval timer but forget to stop it OR you keep adding event listeners without removing them
3. You have more than one variable referencing the same object. You remove the reference of one without removing the other.
4. A function-level variable is being kept alive because it's being referenced outside of the closure. Thus can't be cleaned up.