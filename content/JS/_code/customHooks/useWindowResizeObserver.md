## useWindowResizeObserver Hook

Depends on useDebouncer

```ts
import { useState, useEffect } from 'react';
import { useDebouncer } from './useDebouncer';

export interface IWindowSize {
    width?: number
    height?: number
}

export const useWindowResizeObserver = (callback: () => void, delay = 300) => {

    const [currentWindowSize, setCurrentWindowSize] = useState<IWindowSize>({ width: undefined, height: undefined });
    const debouncedCurrentWindowSize = useDebouncer(currentWindowSize, delay);

    useEffect(() => {
        const handleResize = (e: Event) => {
            let t = (e.target as Window);
            setCurrentWindowSize({ width: t.innerWidth, height: t.innerHeight })
        }
        window.addEventListener('resize', handleResize);
        return () => {
            window.removeEventListener('resize', handleResize);
        };
    }, []);

    useEffect(() => {
        callback();
    }, [debouncedCurrentWindowSize])

}
```
