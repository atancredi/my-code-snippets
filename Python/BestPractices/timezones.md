# ISO Timezone Aware Strings
```python
from datetime import datetime
datetime.now().isoformat() # Timezone naive

# TImezone aware, these are equivalent
datetime.now(tz=timezone.utc).isoformat().replace("+00:00","")[:-3]+'Z' # This requires also from datetime import timezone
datetime.utcnow().isoformat()[:-3]+'Z'
```