# Useful stuff

### check which venv is loaded on shell
```python
import os
print(os.environ["VIRTUAL_ENV"].split("/")[-1])
```


&nbsp;
# Install script
```bash
PYTHON_VERSION="3.10.6"
​
curl https://pyenv.run | bash
echo 'PATH=$HOME/.pyenv/bin:$PATH' >> ~/.bashrc
echo 'eval "$(pyenv init -)"' >> ~/.bashrc
source $HOME/.bashrc
# PYENV VIRTUALENV
git clone https://github.com/pyenv/pyenv-virtualenv.git $(pyenv root)/plugins/pyenv-virtualenv
echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bashrc
source $HOME/.bashrc
​
sudo apt update; sudo apt install build-essential libssl-dev zlib1g-dev \
libbz2-dev libreadline-dev libsqlite3-dev curl llvm \
libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev -y

pyenv install ${PYTHON_VERSION}
pyenv global ${PYTHON_VERSION}
```

# How To Use

```bash
pyenv <command> [<args>]
pyenv help <command>
```

Some useful pyenv commands are as follows.

- **-version** :: Display the version of pyenvcommands List all available pyenv commands
- **exec** :: Run an executable with the selected Python version
- **global** :: Set or show the global Python version(s)
- **help** :: Display help for a command
- **hooks** :: List hook scripts for a given pyenv command
- **init** :: Configure the shell environment for pyenv
- **install** :: Install a Python version using python-build
- **local** :: Set or show the local application-specific Python version(s)
- **prefix** :: Display prefix for a Python version
- **rehash** :: Rehash pyenv shims (run this after installing executables)
- **root** :: Display the root directory where versions and shims are kept
- **shell** :: Set or show the shell-specific Python version
- **shims** :: List existing pyenv shims
- **uninstall** :: Uninstall a specific Python version
- **version** :: Show the current Python version(s) and its origin
- **version-file** :: Detect the file that sets the current pyenv version
- **version-name** :: Show the current Python version
- **version-origin** ::Explain how the current Python version is set
- **versions** ::List all Python versions available to pyenv
- **whence** ::List all Python versions that contain the given executable
- **which** ::Display the full path to an executable