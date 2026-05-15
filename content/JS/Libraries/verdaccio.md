## Run the local server

- in a terminal session start the server
    ```
    npx verdaccio
    ```


- create another terminal session and register the user on the local server
    ```bash
    npm adduser --registry http://localhost:4873
    ```


## create new components
- create a new component
    ```bash
    npm init
    npm install --save-dev typescript @types/react @types/react-dom react
    npx tsc --init
    ```

- add these config to the `tsconfig.json`
    ```json
    {
        "compilerOptions": {
            "target": "ESNext",
            "jsx": "react-jsx",          /* Vital for React 18+ */
            "module": "ESNext",
            "moduleResolution": "node",
            "declaration": true,         /* Generates .d.ts files so your apps see types */
            "declarationDir": "dist",
            "strict": true,
            "esModuleInterop": true,
            "skipLibCheck": true,
            "allowImportingTsExtensions": true,
            "emitDeclarationOnly": true,
            "verbatimModuleSyntax": false
        },
        "include": ["src"]
    }
    ```

- add this to the `package.json`
    ```json
    {
        "main": "./src/index.ts",
        "types": "./src/index.ts",
        "peerDependencies": {
            "react": ">=18"
        },
        "publishConfig": {
            "registry": "http://localhost:4873"
        }
    }
    ```


- generate folder structure
    ```bash
    mkdir src
    mkdir src/components
    touch src/index.ts
    ```


- write code in the components folder and export everything in the `index.ts` file

- publish
    ```bash
    npm publish
    ```


## use the new component
```bash
npm install @your-username/my-components --registry http://localhost:4873
```


## update existing component

- modify code
- bump the version

    Run `npm version patch` (this changes 1.0.0 to 1.0.1 in your package.json).
- publish

    run `npm publish`.
- update

    in the project run `npm update @your-username/my-components`


## remove a published package
```bash
npm unpublish --force <package-name>
```
