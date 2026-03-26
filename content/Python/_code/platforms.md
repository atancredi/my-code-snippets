## Get system platform


```python
import platform

system_name = platform.system()

if system_name == 'Windows':
    print("windows")
elif system_name == 'Linux':
    print("linux")
elif system_name == 'Darwin':
    print("macOS")
else:
    print(f"Unknown system: {system_name}")
```

