## install `uv`
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
uv --version
```

---

## create new package
```bash
uv init mypackage
cd mypackage
```

Creates:

```
mypackage/
├── pyproject.toml
├── README.md
├── src/
│   └── mypackage/
│       └── __init__.py
└── .python-version
```

---


## python version

Use a specific Python version:

```bash
uv python install 3.12
uv python pin 3.12
```

Check:

```bash
uv python list
```

---

## project virtualenv

Create & sync virtual environment:

```bash
uv sync
```

Run commands inside the environment:

```bash
uv run python
uv run pytest
```

---

## dependencies

Add a runtime dependency:

```bash
uv add requests
```

Add a development dependency:

```bash
uv add --dev pytest
```

Remove a dependency:

```bash
uv remove requests
```

---

## dependency files

`uv` uses **`pyproject.toml`** as the source of truth.

Lockfile:

```
uv.lock
```

Regenerate lockfile:

```bash
uv lock
```

---

## run package

Example project structure:

```
src/mypackage/main.py
```

Run:

```bash
uv run python -m mypackage.main
```

Or add a script in `pyproject.toml`:

```toml
[project.scripts]
mypkg = "mypackage.main:main"
```

Run:

```bash
uv run mypkg
```

---

## build package

```bash
uv build
```

Outputs:

```
dist/
├── mypackage-0.1.0.tar.gz
└── mypackage-0.1.0-py3-none-any.whl
```

---

## publish (PyPI)

```bash
uv publish
```

Test PyPI:

```bash
uv publish --repository testpypi
```

---

## using an existing project

Clone repo, then:

```bash
uv sync
```

Run:

```bash
uv run python your_script.py
```

---

|  pip               | uv           |
| ------------------ | ------------ |
| `pip install`      | `uv add`     |
| `pip freeze`       | `uv lock`    |
| `virtualenv`       | `uv sync`    |
| `pipx run`         | `uv run`     |
| `pip install -e .` | `uv sync`    |

---

## some useful flags

```bash
uv add pkg==1.2.3
uv add "pkg>=1.0,<2.0"
uv run --python 3.11 python
```





## notes

- always commit **`uv.lock`**
- `uv run` > activating venvs
- **much faster** than pip + venv
- based on **PEP 621** (`pyproject.toml`)
