# Managing PiP

## Uninstall all packages in an environment
```
pip freeze --exclude-editable | cut -d "@" -f1 | xargs pip uninstall -y
```
Please beware that any editable packages will not be removed (--exclude-editable) and must be done manually. Run pip freeze after the deletion to se the remaining packages!