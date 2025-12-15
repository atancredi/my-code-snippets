

### 1. Create a new folder and go inside it
```bash
mkdir my-app && cd my-app
```

### 2. Initialize npm project and install dependencies
```bash
npm init -y
npm install react react-dom
npm install --save-dev typescript @types/react @types/react-dom esbuild
```

### 3. Create root `index.html`
Note that the 'src' to the bundle script depends on the routing of the app that serves the dist folder!

If this is served under /app so to import bundle you shall src to /app/bundle.js
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>React App</title>
  </head>
  <body>
    <div id="root"></div>
    <script src="app/bundle.js"></script>
  </body>
</html>
```

### 4. Create react app file `src/index.tsx`
```tsx
import React from 'react';
import { createRoot } from 'react-dom/client';

const App = () => {
  return (
    <div>
      <h1>Hello from React!</h1>
    </div>
  );
};

const container = document.getElementById('root');
const root = createRoot(container!);
root.render(<App />);
```

### 5. Add build script in `package.json`
```json
"scripts": {
  "build": "esbuild src/index.tsx --bundle --outfile=dist/bundle.js --loader:.tsx=tsx"
}
```

### 6. Build app with `npm run build`