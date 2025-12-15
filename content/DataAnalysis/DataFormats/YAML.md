
"YAML Ain't Markup Language"
It is not a markup language in the sense that they let you annotate text with formatting or processing instructions mixed with the actual content. This is not the case: YAML is a data serialization format. There is no text in YAML, only data to represent.

It was originally meant to simplify XML but it's much more similar to JSON. The differences between the two formats are an increased emphasis on human readability by adding features on top of JSON.
Thus YAML is more applicable to configuration files edited by hand rather than as a transport layer.

---

### Example of YAML syntax
```yaml
grandparent:
  parent:
    child:
      name: Bobby
    sibling:
      name: Molly
```

or alternatively
```yaml
grandparent:
  parent:
    child: {name: Bobby}
    sibling: {'name': "Molly"}
```

The three fundamental **data structures** in YAML are essentially the same as in [Perl](https://www.perl.org/), which was once a popular scripting language. They’re the following:

1.  **Scalars:** Simple values like numbers, strings, or Booleans
2.  **Arrays:** Sequences of scalars or other collections
3.  **Hashes:** Associative arrays, also known as maps, dictionaries, objects, or records comprised of key-value pairs

You can define a YAML scalar similarly to a corresponding [Python literal](https://docs.python.org/3/reference/lexical_analysis.html#literals). Here are a few examples:

| Data Type      | YAML                                         |
| -------------- | -------------------------------------------- |
| Null           | `null`, `~`                                  |
| Boolean        | `true`, `false`                              |
| Integer        | `10`, `0b10`, `0x10`, `0o10`                 |
| Floating-Point | 3.14`, `12.5e-9`, `.inf`, `.nan              |
| String         | `Lorem ipsum`                                |
| Date and Time  | `2022-01-16`, `23:59`, `2022-01-16 23:59:59` |

## Lists

```yaml
fruits: [apple, banana, orange]
veggies:
  - tomato
  - cucumber
  - onion
mushrooms:
- champignon
- truffle
```

## Python-like dicts
```yaml
person:
  firstName: John
  lastName: Doe
  dateOfBirth: 1969-12-31
  married: true
  spouse:
    firstName: Jane
    lastName: Smith
  children:
    - firstName: Bobby
      dateOfBirth: 1995-01-17
    - firstName: Molly
      dateOfBirth: 2001-05-14
```

## Type Casting
```yaml
text: !!str 2022-01-16

numbers: !!set
  ? 5
  ? 8
  ? 13

image: !!binary
  R0lGODdhCAAIAPAAAAIGAfr4+SwAA
  AAACAAIAAACDIyPeWCsClxDMsZ3CgA7

pair: !!python/tuple
  - black
  - white

center_at: !!python/complex 3.14+2.72j

person: !!python/object:package_name.module_name.ClassName
  age: 42
  first_name: John
  last_name: Doe
```

This translates in the following Python dictionary:
```python
{
    "text": "2022-01-16",
    "numbers": {8, 13, 5},
    "image": b"GIF87a\x08\x00\x08\x00\xf0\x00…",
    "pair": ("black", "white"),
    "center_at": (3.14+2.72j),
    "person": <package_name.module_name.ClassName object at 0x7f08bf528fd0>
}
```

## Anchors and aliases
Other powerful features of YAML are **anchors and aliases**, which let you define an element once and then refer to it many times within the same document. Potential use cases include:

-   Reusing the shipping address for invoicing
-   Rotating meals in a meal plan
-   Referencing exercises in a training program

To declare an anchor, which you can think of as a named variable, you’d use the [ampersand (`&`)](https://en.wikipedia.org/wiki/Ampersand) symbol, while to dereference that anchor later on, you’d use the [asterisk (`*`)](https://en.wikipedia.org/wiki/Asterisk) symbol:

```yaml
recursive: &cycle [*cycle]

exercises:
  - muscles: &push-up
      - pectoral
      - triceps
      - biceps
  - muscles: &squat
      - glutes
      - quadriceps
      - hamstrings
  - muscles: &plank
      - abs
      - core
      - shoulders

schedule:
  monday:
    - *push-up
    - *squat
  tuesday:
    - *plank
  wednesday:
    - *push-up
    - *plank
```

**Note:** Unlike plain XML and JSON, which can only represent [tree-like hierarchies](https://en.wikipedia.org/wiki/Tree_structure) with a single root element, YAML also makes it possible to describe [directed graph](https://en.wikipedia.org/wiki/Directed_graph) structures with [recursive](https://realpython.com/python-recursion/) cycles. Cross-referencing in XML and JSON can be possible, though, with the help of custom extensions or dialects.

You can also **merge** (`<<`) or override attributes defined elsewhere by combining two or more objects:

```yaml
shape: &shape
  color: blue

square: &square
  a: 5

rectangle:
  << : *shape
  << : *square
  b: 3
  color: green
```

The `rectangle` object inherits properties of `shape` and `square` while adding a new attribute, `b`, and changing the value of `color`.

## Preserve newlines literally
the pipe (`|`) indicator placed right after the property name preserves the newlines literally, which can be handy for embedding [shell scripts](https://en.wikipedia.org/wiki/Shell_script) in your YAML file:
```yaml
script: |
   #!/usr/bin/env python

   def main():
       print("Hello world")

   if __name__ == "__main__":
       main()
```

---
# Implementazione in Python
```bash
pip install pyyaml
```

```python
import yaml

with open("example.yaml", "r") as stream:
    try:
        print(yaml.safe_load(stream))
    except yaml.YAMLError as exc:
        print(exc)
```