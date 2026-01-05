

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

## Mobile reset
```css
body, html {
	/* remove page scrolling*/
	overflow: hidden; 

	/* this sets the mobile navbar to black */
	background: #000;
}
```

## Reset user text selection
```css
/*
Source - https://stackoverflow.com/a
Posted by Blowsie, modified by community. See post 'Timeline' for change history
Retrieved 2026-01-03, License - CC BY-SA 4.0
*/

.noselect {
  -webkit-user-select: none;
  user-select: none;
}

```