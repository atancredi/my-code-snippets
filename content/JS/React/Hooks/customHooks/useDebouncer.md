## useDebouncer Hook

```ts
import { useState, useEffect } from 'react';

export const useDebouncer = <T>(inputValue: T, delay: number) => {

    const [debouncedValue, setDebouncedValue] = useState<T>(inputValue)

    useEffect(() => {
        const handler = setTimeout(()=> setDebouncedValue(inputValue), delay);
        return () => clearTimeout(handler);
    }, [inputValue, delay])

    return debouncedValue;

}
```
