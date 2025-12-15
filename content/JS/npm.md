
# Generate project's dependency tree
```shell
npm ls --all
```

## generate tree of a specific dependency
```shell
npm ls [dependency] --depth=[depth]
```

# Find unused dependencies in a project
```shell
npx depcheck 
```

# Visualize bundle of Vite project
```shell
npx vite-bundle-visualizer
```


per stare sicuro che ci siano sempre tutte le dipendenze nei node modules: cancellare la cartella ‘node_modules’, fare ‘npm install’ e poi buildare per vedere se ci sono errori o cose strane (missing dependencies ecc ecc)