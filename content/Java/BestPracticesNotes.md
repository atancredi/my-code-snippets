# Difference between a == b and a.Equals(b)
Assuming the types of `a` and `b` are reference types:

-   In Java, == will always compare for _identity_ - i.e. whether the two values are references to the same object. This is also called _reference equality_. Java doesn't have any user-defined operator overloading.
    
-   In C# it depends. Unless there's an overloaded operator which handles it, == will behave like Java (i.e. comparing for reference equality). However, if there's an overload which matches the _compile-time_ types of `a` and `b` (e.g. if they're both declared as strings) then that overload will be called. That can behave how it wants, but it typically implements _value equality_ (i.e. `a` and `b` can refer to different but _equal_ values and it would still return true).
    

In both languages, `a.Equals(b)` or `a.equals(b)` will call the virtual `Equals`/`equals` method declared by `Object`, unless a more specific overload has been introduced by the compile-time type of `a`. This may or may not be overridden in the execution-time type of the object that `a` refers to. In both .NET and Java, the implementation in `Object` also checks for identity. Note that this depends on the _execution-time type_ rather than the _compilation-time type_ that overload resolution depends on.

Of course, if `a` is `null` then you'll get a `NullReferenceException`/`NullPointerException` when you try to call `a.equals(b)` or `a.Equals(b)`.