
```python
from datetime import datetime, timezone
```


### ISO format
```python
iso_string = '2024-03-01 10:28:29.308000+00:00'

# Use fromisoformat for the most reliable parsing of offsets
dt_object = datetime.fromisoformat(iso_string)

print(dt_object) 
# Output: 2024-03-01 10:28:29.308000+00:00
```


### Custom formats
```python
string = "2024-03-01 10:28:29.308000 UTC"

# make it timezone-aware
clean_string = date_string.replace(" UTC", "")
dt_aware = datetime.strptime(clean_string, "%Y-%m-%d %H:%M:%S.%f").replace(tzinfo=timezone.utc)

print(dt_aware)
```


### Flexible script
```python
formats = [
    "%Y-%m-%d %H:%M:%S.%f %Z",  # Matches '... UTC'
    "%Y-%m-%d %H:%M:%S",        # Matches standard SQL style
    "%d/%m/%Y %H:%M:%S",        # Matches European style
    "%Y-%m-%d %H:%M:%S.%f"
]

def flexible_parse(date_str, formats):
    date_str = date_str.strip()
    for fmt in formats:
        try:
            return datetime.strptime(date_str, fmt)
        except (ValueError, TypeError):
            continue
    
    try:
        return datetime.fromisoformat(date_str)
    except (ValueError, TypeError):
        pass

    raise ValueError(f"Data string '{date_str}' does not match any provided formats.")
```
