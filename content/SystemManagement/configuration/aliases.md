# apply aliases to bashrc


### copy this lines into the `~/.bashrc` file and update it with source `~/.bashrc`
```bash
alias ll='ls -lc --color=auto'

# fuck i forgot to add sudo
alias fuck='sudo \$(history -p \!\!)'

# please install ahhahahah
alias please='sudo apt-get'

# search a command in the history
alias cmd='history | grep'

# make a script executable
alias exe='chmod u+x'

# search running command
alias proc='ps -aux | grep '
```
