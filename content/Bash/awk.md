
typically used after commands returning complex text output (hence the pipe character at the beginning of the lines below)

## print first token (splitting by whitespace)
```bash
| awk '{print $0}'
```


&nbsp;
## last line
```bash
| awk 'END{print}'
```