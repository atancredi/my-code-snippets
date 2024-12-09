
## List running processes
this works in restricted shells such as docker inner terminal (when you can't use ps or top)
```bash
ls -l /proc/*/exe
```

&nbsp;
## Make a script actually executable
```bash
chmod u+x <filename>
```

&nbsp;
## Remove a path from exported \$PATH variable
```bash
export PATH=${PATH%:/path/to/folder}
```
