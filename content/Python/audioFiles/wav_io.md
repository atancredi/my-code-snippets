# I/O operations on wav files

```python
import numpy as np
from scipy.io.wavfile import write

rate = 44100
scaled = np.int16(data / np.max(np.abs(data)) * 32767)
write('test.wav', rate, scaled)
```
