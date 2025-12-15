# best practices
1. **1.Prefer Generics:** Whenever possible, use generics to ensure type safety. Generics provide a way to write code that is both flexible and strongly typed.
2. **2.Use** instanceof for Downcasting: Before performing downcasting, use the instanceof operator to check the compatibility of the object with the target type. This helps prevent runtime errors.
3. **3.Avoid Unchecked Cast Warnings:** Be cautious when suppressing unchecked cast warnings. Instead, try to refactor the code to eliminate the need for such suppressions.
4. **4.Understand the Type Hierarchy:** A clear understanding of the class hierarchy is crucial for effective casting. Ensure that the casting operations align with the inheritance relationships in your code.
5. **5.Use Wrapper Classes for Primitive Types:** When dealing with primitive types, consider using wrapper classes (e.g., Integer, Double) and their methods for conversions to and from strings. This approach provides safety and avoids potential pitfalls associated with primitive casting.
6. **6.Keep Code Readable:** Excessive casting can make code difficult to read and maintain. If your design requires frequent casting, it might be worth revisiting the class hierarchy or considering alternative design patterns.

### upcasting (OK):
converting an object of a subclass to its superclass  -> ok because it's actually performed by the compiler (every instance of a subclass is also an instance of its superclass)
```java
Superclass superObj = new Subclass();
```

superObj is a reference to an object of the superclass that actually points to an instance of the subclass.

### downcasting (BAD):
- converting an object of a superclass to its subclass
downcasting requires an explicit cast and may lead to runtime errors if not done carefully

##### try to avoid downcasting to a type or interface that is not guaranteed

**Generics, lets us do this:
```java
private static <T extends Speakable & Flyer> void reallySafeSpeakAndFly(T x) {
    x.Speak();
    x.Fly();
}
```

Here, the compiler can make sure we're not passing something that doesn't implement `Speakable` and `Flyer` and can detect such sassy attempts at compile-time.**

### implicit casting
supported for primitive types, when converting from a 'smaller' data type to a 'larger' one, such as writing an 'int' number inside a 'double' variable (because it's wider -> this method is also called widening)
```java
int v = 1;
double vv = v; // ok
```

### explicit casting
when converting from a 'larger' data type to a 'smaller' one we need to cast explicitly:
```java
double v = 1.0;
int vv = (int) v; // ok
```

### dynamic casting (using instanceof)
used especially for downcasting. The concept is checking if an object is actually an instance of that class before casting it: this approach avoids ClassCastExceptions.\

```java
if (superObj instanceof Subclass) {
	Subclass subObj = (Subclass) superObj;
}
```



---
sources
- https://www.linkedin.com/pulse/mastering-casting-java-comprehensive-guide-kalana-heshan-5pcmc/