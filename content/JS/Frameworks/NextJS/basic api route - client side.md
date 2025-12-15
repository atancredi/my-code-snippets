```tsx
import { useEffect, useState } from "react";
import axios from "axios";

export default function Home() {
	const [data, setData] = useState(null);

	useEffect(() => {
		axios.get('/api/<endpoint>')
			.then((response) => {
				setData(response.data);
			})
	}, []);

	return (
		<div>
			{JSON.stringify(data)}
		</div>
	);
}
```