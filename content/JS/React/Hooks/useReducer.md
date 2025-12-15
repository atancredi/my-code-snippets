## useReducer Hook

```ts
interface IndexableObject {
    [key: string]: any
}

const [obj, setObj] = useReducer(
    (state: IndexableObject, newState: IndexableObject) => {
        return {...state, ...newState}
    }, {}
);
```
