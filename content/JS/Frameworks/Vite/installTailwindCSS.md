# Install Tailwind on a Vite project

### Install
```shell
npm install tailwindcss @tailwindcss/vite
```

### Configure vite.config.ts
```ts
import { defineConfig } from 'vite'
import tailwindcss from '@tailwindcss/vite'
export default defineConfig({ 
	plugins: [
		tailwindcss(),
	],
})
```

### Import tailwind to the global CSS file
```css
@import "tailwindcss";
```
