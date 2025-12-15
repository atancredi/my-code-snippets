

## General page reset

```css
html {
	box-sizing: border-box
}

body {
	margin: 0;
}

*, *::before, *::after {
	box-sizing: inherit
}
```

## Single elements resets

### Button
Generally i still want the pointer cursor and an hover effect to make it feel clickable...
```css
button {
	all: unset;
    cursor: pointer;
}

button:hover {
    text-decoration: underline;
}
```
