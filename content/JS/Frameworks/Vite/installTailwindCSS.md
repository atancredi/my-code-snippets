# Install Tailwind on a Vite project

### Install
```shell
npm install tailwindcss @tailwindcss/vite
```

### Configure vite.config.ts
```ts
import react from '@vitejs/plugin-react'
import { defineConfig } from 'vite'
import tailwindcss from '@tailwindcss/vite'

// https://vite.dev/config/
export default defineConfig({ 
	plugins: [
    	react(),
		tailwindcss(),
	],
})
```

### Import tailwind to the global CSS file
```css
@import "tailwindcss";
```
