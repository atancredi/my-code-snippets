To use an infinite loop with tqdm you need to change your while loop into an infinite for loop by utilizing a generator.

Infinite loop (no progress bar)

```python
while True:
  # Do stuff here
Infinite loop (with progress bar)

def generator():
  while True:
    yield

for _ in tqdm(generator()):
  # Do stuff here
```
The code above would create an indefinite progress bar that would look similar to this

```
16it [01:38,  6.18s/it]
```

Note that the generator could also be modified to work with a condition

```python
def generator():
  while condition:
    yield
```

[Source](https://stackoverflow.com/questions/45808140/using-tqdm-progress-bar-in-a-while-loop)