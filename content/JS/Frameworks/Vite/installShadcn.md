

```bash
npm install -D tailwindcss@3.4.17 postcss autoprefixer
```

```bash
npx tailwindcss init -p
```


## Add this import header in your main css file, src/index.css in our case:
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

## Configure the tailwind template paths in tailwind.config.js:
```ts
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./index.html", "./src/**/*.{ts,tsx,js,jsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

## Edit tsconfig.json file
The current version of Vite splits TypeScript configuration into three files, two of which need to be edited. Add the baseUrl and paths properties to the compilerOptions section of the tsconfig.json and tsconfig.app.json files:
```json
{
  "files": [],
  "references": [
    {
      "path": "./tsconfig.app.json"
    },
    {
      "path": "./tsconfig.node.json"
    }
  ],
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    }
  }
}
```

## Edit tsconfig.app.json file
Add the following code to the tsconfig.app.json file to resolve paths, for your IDE:
```json
{
  "compilerOptions": {
    // ...
    "baseUrl": ".",
    "paths": {
      "@/*": [
        "./src/*"
      ]
    }
    // ...
  }
}
```


## Update vite.config.ts
```bash
npm install -D @types/node
```
```ts
import path from "path"
import react from "@vitejs/plugin-react"
import { defineConfig } from "vite"

export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: {
      "@": path.resolve(__dirname, "./src"),
    },
  },
})
```

```bash
npx shadcn@latest init
```

That's it
