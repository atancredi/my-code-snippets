## Renaming files in a folder to sequential numbers
```bash
ls -v | cat -n | while read n f; do mv -n "$f" "$n.wav"; done
```
