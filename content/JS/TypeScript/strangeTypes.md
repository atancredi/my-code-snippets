# What to do with strange types?

### Return type of a function

#### Use Case:
React hooks that returns an object without an interface, and when the hook is coming from a library is difficult to annotate the type in the code base (could lead to update issues if the library diverges in some way).

A way to refer to the return type of a function is, generally:
```ts
type HookReturn = ReturnType<typeof useMyHook>
```
