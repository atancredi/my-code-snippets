# Hide/show scrollbar on an element

```css
/*
Source - https://stackoverflow.com/a/38994837
Posted by Hristo Eftimov, modified by community. See post 'Timeline' for change history
Retrieved 2025-12-16, License - CC BY-SA 4.0
*/

.hide-scrollbar {
    -ms-overflow-style: none;  /* Internet Explorer 10+ */
    scrollbar-width: none;  /* Firefox, Safari 18.2+, Chromium 121+ */
}
.hide-scrollbar::-webkit-scrollbar { 
    display: none;  /* Older Safari and Chromium */
}

```

## minimal custom scrollbar
```css
.minimal-scrollbar {
    scrollbar-width: thin;
    scrollbar-color: rgba(0, 0, 0, 0.45) transparent;
}
.minimal-scrollbar::-webkit-scrollbar {
    width: 6px;
    height: 6px;
}
.minimal-scrollbar::-webkit-scrollbar-track {
    background: transparent;
}
.minimal-scrollbar::-webkit-scrollbar-thumb {
    background-color: rgba(0, 0, 0, 0.45);
    border-radius: 999px;
    transition: background-color 0.2s ease;
}
.minimal-scrollbar::-webkit-scrollbar-thumb:hover {
    background-color: rgba(0, 0, 0, 0.65);
}
.minimal-scrollbar::-webkit-scrollbar-thumb:active {
    background-color: rgba(0, 0, 0, 0.8);
}
```
