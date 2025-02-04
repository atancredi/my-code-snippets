# useEffect

It's the alternative for the class component lifecycle methods `componentDidMount`, `componentWillUnmount`, `componentDidUpdate`, etc. You can also use it to create a side effect when dependencies change, i.e. "If some variable changes, do this".

# useCallback

On every render, everything that's inside a functional component will run again. If a child component has a dependency on a function from the parent component, the child will re-render every time the parent re-renders even if that function "doesn't change" (the reference changes, but what the function does won't).  
It's used for optimization by avoiding unnecessary renders from the child, making the function change the reference only when dependencies change. You should use it when a function is a dependency of a side effect e.g. `useEffect`.

# useMemo

It will run on every render, but with cached values. It will only use new values when certain dependencies change. It's used for optimization when you have expensive computations. [Here is also a good answer that explains it](https://stackoverflow.com/a/55418085/9119186).


(https://stackoverflow.com/questions/56910036/when-to-use-usecallback-usememo-and-useeffect)