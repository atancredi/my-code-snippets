# Run unit tests with pytest

```bash
uv add pytest
```

## Naming Conventions (Test Discovery)
Pytest automatically finds tests by searching your project for files and functions that match specific naming patterns:

Files: Must be named test_*.py or *_test.py (e.g., test_calculator.py).

Functions: Must start with test_ (e.g., def test_addition():).

Classes (optional): Must start with Test and have no __init__ method.

## Writing Your First Test
Pytest uses standard Python assert statements. You don't need to learn a new assertion API (like self.assertEqual in unittest).


```python
# test_math.py
def add(a, b):
    return a + b

def test_add_positive_numbers():
    assert add(2, 3) == 5

def test_add_negative_numbers():
    assert add(-1, -1) == -2
```

## Running Tests (CLI)
Run tests from your terminal in the root directory of your project:

Run all tests: `pytest`

Run with verbose output: `pytest -v`

Run a specific file: `pytest test_math.py`

Run a specific test function: `pytest test_math.py::test_add_positive_numbers`

Run tests matching a keyword: `pytest -k "negative"` (runs tests with "negative" in the name)

Stop after the first failure: `pytest -x`


## Testing Exceptions
Use `pytest.raises` as a context manager to verify that your code throws the correct exceptions.

```python
import pytest

def divide(a, b):
    if b == 0:
        raise ValueError("Cannot divide by zero")
    return a / b

def test_divide_by_zero():
    with pytest.raises(ValueError) as exc_info:
        divide(10, 0)
    
    # Optional: check the error message
    assert str(exc_info.value) == "Cannot divide by zero"
```

## Fixtures (Setup & Teardown)
Fixtures are Pytest's way of managing setup, teardown, and dependency injection. They provide reusable baseline states or data for your tests.

```Python
import pytest

# Define the fixture
@pytest.fixture
def sample_user():
    user = {"username": "johndoe", "email": "john@example.com"}
    return user

# Inject the fixture by passing its name as an argument
def test_user_email(sample_user):
    assert sample_user["email"] == "john@example.com"

# Setup and Teardown using 'yield'
@pytest.fixture
def database_connection():
    # Setup: Create connection
    db = "Connected"
    
    yield db  # Provide the fixture to the test
    
    # Teardown: Close connection (runs after the test finishes)
    db = "Disconnected" 
```

## Parametrization (Data-Driven Testing)
Instead of writing multiple tests for different input values, use @pytest.mark.parametrize to run the same test with a list of inputs.

```Python
import pytest

def is_even(n):
    return n % 2 == 0

@pytest.mark.parametrize("number, expected", [
    (2, True),
    (4, True),
    (3, False),
    (0, True),
    (-1, False)
])
def test_is_even(number, expected):
    assert is_even(number) == expected
```

## Markers (Skipping and Expected Failures)
Markers allow you to add metadata to your tests. The most common built-in markers are for skipping tests or marking them as expected to fail.

```Python
import pytest
import sys

# Skip a test unconditionally
@pytest.mark.skip(reason="Feature not implemented yet")
def test_future_feature():
    assert False

# Skip a test based on a condition
@pytest.mark.skipif(sys.platform == "win32", reason="Does not run on Windows")
def test_linux_only_feature():
    assert True

# Mark a test that you know is broken (prevents the test suite from failing)
@pytest.mark.xfail(reason="Bug #123 waiting on a fix")
def test_known_bug():
    assert 1 == 2
```

---
generated with Gemini 3.1 Pro