# Folder Structure
Example repository: [available on GitHub](https://github.com/kennethreitz/samplemod)
```
README.rst
LICENSE
setup.py
requirements.txt
sample/__init__.py
sample/core.py
sample/helpers.py
docs/conf.py
docs/index.rst
tests/test_basic.py
tests/test_advanced.py
```

## The actual module
	`./sample/`	
Folder containing the code of interest. If only a single file is needed this is equivalent:
	`./sample.py`

## LICENCE, README
Both `.md` or MarkDown files, LICENCE contains the licence used for the project and it is fundamental for other developers to use or contribute to the code.
The README file contains basic informations about the project and usually it provides a quick tutorial on how to set up and run the software.

## Setup.py
A file purposed for package and distribution management. It should be at the same folder level of the module package, so if it's located at the root of the repository, the setup file should also be.

## Requirements.py
Pip requirements file, should be placed at the root of the repository. It should specify the dependencies required to contribute or run the project. This file may be unnecessary if dependencies are handled by `setup.py` or in other ways.

## Documentation
	`./docs`
Package reference documentation. It's not always needed.


# Code Structure
## Poorly structured projects
- multiple circular dependencies. If the classes Table and Chair in `furn.py` need to import Carpenter from `workers.py` to answer a question such as `table.isdoneby()`, and if conversely the class Carpenter needs to import Table and Chair to answer the question `carpenter.whatdo()`, then you have a circular dependency.
- Hidden coupling: Each and every change in Table’s implementation breaks 20 tests in unrelated test cases because it breaks Carpenter’s code, which requires very careful surgery to adapt to the change. This means you have too many assumptions about Table in Carpenter’s code or the reverse.
- Heavy usage of global state or context
- Spaghetti code: multiple pages of nested if clauses and for loops with a lot of copy-pasted procedural code and no proper segmentation are known as spaghetti code. Python’s meaningful indentation (one of its most controversial features) makes it very hard to maintain this kind of code. The good news is that you might not see too much of it.
- Ravioli code is more likely in Python: it consists of hundreds of similar little pieces of logic, often classes or objects, without proper structure. If you never can remember, if you have to use FurnitureTable, AssetTable or Table, or even TableNew for your task at hand, then you might be swimming in ravioli code.


# Modules
To keep in line with the style guide, keep module names short, lowercase, and be sure to avoid using special symbols like the dot (.) or question mark (?). A file name like `my.spam.py` is the one you should avoid! Naming this way will interfere with the way Python looks for modules.

If you like, you could name your module `my_spam.py`, but even our trusty friend the underscore, should not be seen that often in module names. However, using other characters (spaces or hyphens) in module names will prevent importing (- is the subtract operator). Try to keep module names short so there is no need to separate words. And, most of all, don’t namespace with underscores; use submodules instead.
``` python
# OK
import library.plugin.foo
# not OK
import library.foo_plugin
```

In many languages, an `include file` directive is used by the preprocessor to take all code found in the file and ‘copy’ it into the caller’s code. It is different in Python: the included code is isolated in a module namespace, which means that you generally don’t have to worry that the included code could have unwanted effects, e.g. override an existing function with the same name.

It is possible to simulate the more standard behavior by using a special syntax of the import statement: `from modu import *`. This is generally considered bad practice. **Using** `import *` **makes the code harder to read and makes dependencies less compartmentalized**.

Using `from modu import func` is a way to pinpoint the function you want to import and put it in the local namespace.

**Very Bad**
```python
from modu import *
x = sqrt(4)  # Is sqrt part of modu? A builtin? Defined above?
```

**Better**
```python
from modu import sqrt
x = sqrt(4)  # sqrt may be part of modu, if not redefined in between
```

**Best**
``` python
import modu
x = modu.sqrt(4)  # sqrt is visibly part of modu's namespace
```

Being able to tell immediately where a class or function comes from, as in the `modu.func` idiom, greatly improves code readability and understandability in all but the simplest single file projects.

# Packages
Any directory with an `__init__.py` file is considered a Python package. The different modules in the package are imported in a similar manner as plain modules, but with a special behaviour for the `__init__.py` file, which is used to gather all package-wide definitions.

Leaving an `__init__.py` file empty is considered normal and even good practice, if the 
package’s modules and sub-packages do not need to share any code.

Lastly, a convenient syntax is available for importing deeply nested packages: `import very.deep.module as mod`. This allows you to use mod in place of the verbose repetition of `very.deep.module`.
