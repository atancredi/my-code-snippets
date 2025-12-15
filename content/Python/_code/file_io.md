# File I/O in Python

## Generate all files in a directory with certain extensions
```python
def files_in_folder(directory, extensions):
    extensions = [ext.lower() for ext in extensions]
    for root, _, files in os.walk(directory):
        for file in files:
            file_extension = os.path.splitext(file)[1].lower()
            if file_extension in extensions:
                yield os.path.join(root, file)
```

## Walk a directory
```python
import os

root_path = '/your/start/directory'  # Replace with your directory path

for dirpath, dirnames, filenames in os.walk(root_path):
    print(f"Currently in directory: {dirpath}")
    
    for dirname in dirnames:
        full_dir_path = os.path.join(dirpath, dirname)
        if os.path.isdir(full_dir_path):
            print(f"  Directory: {full_dir_path}")
    
    for filename in filenames:
        full_file_path = os.path.join(dirpath, filename)
        if os.path.isfile(full_file_path):
            print(f"  File: {full_file_path}")
```

## Get directory name from a file's path or glob
If using Python 3.4 or above use PathLib.

```python
# using OS
import os
path=os.path.dirname("C:/folder1/folder2/filename.xml")
print(path)
print(os.path.basename(path))

# using pathlib
import pathlib
path = pathlib.PurePath("C:/folder1/folder2/filename.xml")
print(path.parent)
print(path.parent.name)
```