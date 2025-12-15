# How to manage testing inside package
```
tests/test_basic.py
tests/test_complex.py
tests/context.py # Create an import context for the text
```
#### Context.py:
```
import os
import sys
sys.path.insert(0, os.path.abspath(os.path.join(os.path.dirname(__file__), '..')))

import sample # Package Name
```

Then, within the individual test modules, import the module like so:
`from .context import sample`
This will always work as expected, regardless of installation method.

It's also important to note that it's not always possible to distribute tests within the module itself: additional and heavy dependencies may be required, so it's useless to include them in the actual module.


---
https://docs.python-guide.org/writing/tests/
