# Manage env variables

```python
from os import environ as env
from typing import Optional, Any, Type

from dotenv import load_dotenv


class EnvironmentLoader:

    def __init__(self, dotenv_path: Optional[str] = None):
        load_dotenv(dotenv_path=dotenv_path)

    def get(self, key: str, default: Optional[Any] = None, t: Type = str):
        v = env.get(key)
        if v is not None:
            return t(v)
        elif default is None:
            raise KeyError(f"Required env var with key {key} not found.")
        else:
            return default

```

requires:
```
python-dotenv
```
