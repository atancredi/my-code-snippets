
## Add a submodule
```bash
git submodule add https://github.com/tuo-utente/tuo-repo-componente.git components/tuo-componente
```

## Manually update all submodules
```bash
git submodule update --remote --merge
```

## Clone project including all submodules
Used when cloning on another machine
```bash
git clone --recurse-submodules https://github.com/tuo-utente/tuo-progetto-principale.git

# if the project was already cloned without the submodules
git submodule update --init --recursive

```

## Update a single submodule
```bash
# go in the submodule folder
cd components/componente1

# checkout to the latest branch and pull changes
git checkout main
git pull origin main

# go back to the root repo and commit the updates
cd ../..  
git add components/componente1
git commit -m "Aggiornato componente1"
git push origin main
```

## Permanently remove a submodule
```bash
# Remove the submodule entry from .git/config
git submodule deinit -f path/to/submodule

# Remove the submodule directory from the superproject's .git/modules directory
rm -rf .git/modules/path/to/submodule

# Remove the entry in .gitmodules and remove the submodule directory located at path/to/submodule
git rm -f path/to/submodule
```

## Track changes on a submodule
```bash
# if a '+' appears in front of a commit, it must be updated
git submodule status

# to see the differences
git diff --submodule
```

## Rollback to a previous version
```bash
# go to the submodule folder
cd components/componente1

# look at the history logs
git log --oneline

# checkout to a previous commit (by commit id)
git checkout <commit_id>

# go back to the root repo and commit the updates
cd ../..
git add components/componente1
git commit -m "Ripristinato componente1 a versione precedente"
git push origin main
```

