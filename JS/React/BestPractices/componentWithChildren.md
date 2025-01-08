## Best practice for creating a custom component with children in react:

```js
import { PropsWithChildren } from "react";

export interface CustomComponentProps extends PropsWithChildren {
    className?: string
}

export function CustomComponent (props: CustomComponentProps) {
    return (<div className={props.className}>
        {props.children}
    </div>)
}
```
