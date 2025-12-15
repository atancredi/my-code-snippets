# Memory_Profiler

https://pypi.org/project/memory-profiler/

```
mprof run --multiprocess <script>
mprof plot
```

## view memory usage slope
- >0 it might mean a memory leak.
- ~0 if 0 or near 0, the memory usage may be considered stable.
- <0 to be interpreted depending on the expected process memory usage patterns, also might mean that the sampling period is too small.
```
mprof plot -s
```

---
### tools
https://pypi.org/project/line-profiler/2.1.2/
https://pypi.org/project/memory-profiler/
https://github.com/bloomberg/memray fighissimo anche live pd

https://pyinstrument.readthedocs.io/en/latest/guide.html#profile-a-web-request-in-fastapi

https://bruinsslot.jp/post/profiling-python-docker/

https://uwpce-pythoncert.github.io/SystemDevelopment/profiling.html

https://freedium.cfd/https://towardsdatascience.com/how-to-assess-your-code-performance-in-python-346a17880c9f

load testing https://www.techtarget.com/searchsoftwarequality/tip/What-to-know-about-Python-load-testing


&nbsp;
# Boosting Python performance
https://www.theserverside.com/tip/Tips-to-improve-Python-performance


There are ways to better structure your Python code to improve performance

### A few key approaches

- Overhead in function/method runtime lookup can be significant for small frequent calls.
- inlining code or caching function references might help. See `examples/data_aggregation/agg.py`
- Python string handling idioms: use `"".join(list_of_strings)` rather than sequential calls to += See `examples/strings/str_concat.py` and `str_comprehensions.py`
- using list comprehensions, generator expressions, `or map()` instead of for loops can be faster (see `data_aggregation/loops.py`)
- Leverage existing domain specific C extension libraries, for instance Numpy for fast array operations.
- Rewrite expensive code as C modules. Use ctypes, Cython, SWIG, ...

[http://wiki.python.org/moin/PythonSpeed/PerformanceTips/](http://wiki.python.org/moin/PythonSpeed/PerformanceTips/)