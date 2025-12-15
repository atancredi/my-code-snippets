```js
import { Inter } from 'next/font/google'

export default function RootLayout({ children,}: { children: React.ReactNode}) {
	return ( <html lang="en" className={inter.className}>
		<body>{children}</body>
	</html> )
}
```