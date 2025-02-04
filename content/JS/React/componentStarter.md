# client component with state
```jsx
"use client";
import { useEffect, useState } from 'react';

export default function Component(){
	const [isLoaded, setIsLoaded] = useState(false);
	
	useEffect(() => {
		setIsLoaded(true);
	}, []);
	// Just spins
	return (<div></div>)
}
```