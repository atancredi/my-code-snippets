
```css
:root {
  font-family: Inter, system-ui, Avenir, Helvetica, Arial, sans-serif;
  line-height: 1.5;
  font-weight: 400;

  font-synthesis: none;
  text-rendering: optimizeLegibility;
  --webkit-font-smoothing: antialiased;
  --moz-osx-font-smoothing: grayscale;
}
```



### font-synthesis: none;
This property controls whether the browser is allowed to synthesize bold or italic styles if they are missing from the font.
- none: Prevents the browser from generating fake bold (\<b>) or italic (\<i>) styles when the font does not provide them.

### text-rendering: optimizeLegibility;

This property enhances text readability by enabling features like:
- Ligatures (proper character connections)
- Kerning (spacing between characters)
- Improved font hinting for better clarity

### -webkit-font-smoothing: antialiased;

This is a WebKit-specific property (for Chrome, Safari) that smooths font edges for better readability.
- antialiased makes text look thinner and sharper by improving font rendering.

### -moz-osx-font-smoothing: grayscale;

This is a Firefox/macOS-only property that smooths text using grayscale anti-aliasing.
- grayscale: Reduces color fringing and makes fonts appear sharper.
