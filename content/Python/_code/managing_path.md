# Path stuff in Python

## Extract file name from path
```python
import os
print(os.path.basename(your_path))
```

## Extract common prefix and relative paths for a list of paths
```python
from os.path import commonpath, relpath
# expected: /path/to/folder
common_prefix = commonpath(['/path/to/folder', '/path/to/folder/to/file'])
rel_path = relpath(x, common_prefix)
# expected: /path/to/folder, /to/file
print(common_prefix, rel_path)
```