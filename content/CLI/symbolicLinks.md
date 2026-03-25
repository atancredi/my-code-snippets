
# Create a symbolic link to a folder

### Linux
```bash
ln -s ./orig ./where/to/put
```

### Windows (PowerShell as admin)
```powershell
New-Item -ItemType SymbolicLink -Path ".\where\to\put" -Target ".\orig"
```
