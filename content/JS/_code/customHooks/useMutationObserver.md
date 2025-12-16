## useDebouncer Hook

```ts
import { RefObject, useEffect } from "react";

export const useMutationObserver = <T extends Element>(
    ref: RefObject<T | undefined>,
    callback: MutationCallback,
    options = {
        attributes: true,
        characterData: true,
        childList: true,
        subtree: true,
    }
) => {
    useEffect(() => {
        if (ref != undefined && ref.current != null) {
            const observer = new MutationObserver(callback);
            observer.observe(ref.current, options);
            return () => observer.disconnect();
        }
    }, [callback, options]);
};

```
