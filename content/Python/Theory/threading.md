# Asyncio


In python multi-threaded code never run at the same time, due to the [GIL](https://wiki.python.org/moin/GlobalInterpreterLock) only one thread is running at a time, so only one core can be used at a time with python threading.
This means that multiple threads will still use only one process, so true parallel processing (e.g. for heavy tasks) can only be obtained with the Multiprocessing module.

### combining asyncio with threading 

as stated in [bpo#34697](https://bugs.python.org/issue34679) when combining `asyncio` with threads, the event loop should generally be run in the main loop [Source](https://stackoverflow.com/questions/68150011/problems-using-asyncio-in-thread)


### Threading

https://stackoverflow.com/questions/45414050/using-asyncio-and-threads

AsyncIO runs in a single thread: in fact asynchronous code in principle runs some routines in the same thread.
When one routine has to wait for I/O it will halt that routine temporally and starts processing another routine until it encounters a wait there, and so on.

### Multiprocessing / Parallel Processing
Opposing to multi-threading, the only way to actually run heavy loads in parallel - allowing python to run on every core of the CPU and in multiple processes, using the Multiprocessing library.

&nbsp;
# ProcessPool vs ThreadPool

It's pretty simple to delegate a method to a thread or sub-process using [`BaseEventLoop.run_in_executor`](https://docs.python.org/3/library/asyncio-eventloop.html#asyncio.BaseEventLoop.run_in_executor):

```python
import asyncio
import time
from concurrent.futures import ProcessPoolExecutor

def cpu_bound_operation(x):
    time.sleep(x) # This is some operation that is CPU-bound

@asyncio.coroutine
def main():
    # Run cpu_bound_operation in the ProcessPoolExecutor
    # This will make your coroutine block, but won't block
    # the event loop; other coroutines can run in meantime.
    yield from loop.run_in_executor(p, cpu_bound_operation, 5)


loop = asyncio.get_event_loop()
p = ProcessPoolExecutor(2) # Create a ProcessPool with 2 processes
loop.run_until_complete(main())
```

As for whether to use a `ProcessPoolExecutor` or `ThreadPoolExecutor`, that's kind of hard to say; pickling a large object will definitely eat some CPU cycles, which initially would make you think `ProcessPoolExecutor` is the way to go. However, passing your 100MB object to a `Process` in the pool would require pickling the instance in your main process, sending the bytes to the child process via IPC, unpickling it in the child, and then pickling it _again_ so you can write it to disk. Given that, my guess is the pickling/unpickling overhead will be large enough that you're better off using a `ThreadPoolExecutor`, even though you're going to take a performance hit because of the GIL.

That said, it's very simple to test both ways and find out for sure, so you might as well do that.

[Source](https://stackoverflow.com/questions/28492103)