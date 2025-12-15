

## Find a path in current folder and subfolders
```bash
find . -iname "obsidian*"
```

## Count number of files in a folder
```bash
ls -1 | wc -l
```

&nbsp;

&nbsp;

&nbsp;

# `PATH` variable

## Add a new path to path variable
```bash
export PATH=/path/to/folder:$PATH
```
This edits the variable temporarily, only in the current bash session.

To add permanently to PATH, add this line to the `.zshrc` or `.bashrc`:
```bash
PATH=<newpath>:$PATH
```

## Remove a path from exported \$PATH variable
```bash
export PATH=${PATH%:/path/to/folder}
```

