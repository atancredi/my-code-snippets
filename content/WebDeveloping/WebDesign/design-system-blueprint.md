# Design system blueprint for responsive web apps


## Typography


### Font sizes

$R$ is the ratio
| token | value |
|--|--|
| sm | $1/R$ |
| base | `1rem` $\cdot R$  |
| lg | $base\cdot R$ |
| xl | $lg\cdot R$ |
| 2xl | $xl\cdot R$ |
| 3xl | $2xl\cdot R$ |
| 4xl | $3xl\cdot R$ |

the formula is 
$$size_n = base \cdot R^n$$


it's good practice to use CSS `clamp()` for giving the text a truly responsive feeling.


### Different ratios

|R|what for|
|--|--|
| 1.2 | **dashboards, dense data visualizations** |
| | steps: `1rem`, `1.2rem`, `1.44rem`, `1.72rem`, `2.07rem` |
| | suggested fonts: `Inter, Roboto, San Francisco` |
| 1.333 | **text-based apps** |
| | steps: `1rem`, `1.33rem`, `1.77rem`, `2.36rem`, `3.15rem` |
| | suggested fonts: `Helvetica, Open Sans, Lato` |


**N.B.** `font-size` steps are `base`, `lg`, `xl`, `2xl`, `3xl`


### Line heights
|ratio|what for|
|--|--|
| 1 | text, buttons, UI elements |
| 1.1 | 3xl, 4xl text |
| 1.25 | lg, xl, 2xl text |
| 1.5 | paragraphs/body text - *minimum value for accessibility rules* |
| 1.6 | large blocks of dense text or sm-text captions |



### Weights

- **400**: standard text
- **500**: buttons, active states, sub-labels
- **700**: bold (headings)


### Tracking (letter spacing)
Smaller text often needs wider tracking for legibility, larger headings need tighter tracking.
```css
--tracking-tight: -0.025em; /* (For headings 2xl and up) */
--tracking-normal: 0; /* (Base) */
--tracking-wide: 0.025em; /* (For uppercase sm labels) */
```

### Fonts

- define a `--mono-font: 'Fira Code', monospace;` token for code blocks, tabular data, strict number values.

## Spacing

| token | what for | example value |
|--|--|--|
|`--space-1`|Tiny UI nudges (4px)|`0.25rem`|
|`--space-2`|Inner component spacing (8px)|`0.5rem`|
|`--space-3`|Tight layout gaps (12px)|`0.75rem`|
|`--space-4`|Standard margin/padding (16px)|`1rem`|
|`--space-6`|Medium layout gaps (24px)|`1.5rem`|
|`--space-8`|Form groupings, section breaks (32px)|`2rem`|
|`--space-12`|Large page sections (48px)|`3rem`|
|`--space-16`|Massive white space, hero padding (64px)|`4rem`|

### Some tips

- *for text paragraphs*: limit width to 65 or 75 characters with `max-width: 65ch;`


## Elevation (Shadows & Z-Index)
Create depth purposefully to show hierarchy (e.g., a dropdown sits "above" a card).

```css
/* Shadows: */
--shadow-sm: ; /* Buttons, inputs (subtle depth) */
--shadow-md: ; /* Cards, dropdown menus */
--shadow-lg: ; /* Modals, popovers, floating action buttons */

/* Z-Index Scale: Prevent the classic z-index: 99999 nightmare by tokenizing them */
--z-elevated: 10;
--z-dropdown: 50;
--z-sticky: 100;
--z-modal: 1000;
```


## Shape (Border Radius)
Consistent rounded corners instantly make an app feel cohesive.

```css
--radius-sm: 0.25rem;  /* Checkboxes, small tags */
--radius-md: 0.5rem;   /* Buttons, inputs, standard cards */
--radius-lg: 1rem;     /* Large modals, floating elements */
--radius-full: 9999px; /* Pills, avatars */
```


## Colors

Define at least 5 main colors: **Primary**, **Neutral/Grey**, **Success**, **Warning**, **Danger**.


For every color define at least 5 shades:
- **50/100**: Subtle backgrounds (success banners, hover on secondary buttons)
- **300/400**: Borders, muted icons, disabled active states
- **500/600**: Core brand colors, primary actions
- **700/800**: Hover states for primary actions
- **900**: High-contrast text on light backgrounds

### Some examples
- **Primary**
    - 100: light active states
    - 500: buttons, links
    - 900: high-contrast accents
- **Neutral/Grey**
    - 100: app background
    - 500: borders, disabled states
    - 900: body text, headings
- **Danger**
    - 100: error banners bg
    - 500: delete buttons
    - 900: error text


## Transitions
Define standard transition times and easing curves to keep animations feeling uniform.
```css
--ease-standard: cubic-bezier(0.4, 0, 0.2, 1);
--duration-fast: 150ms; /* Hover states, color changes */
--duration-base: 300ms; /* Opening modals, expanding accordions */
```


## Coding best practice

### Using theme's tokens
Define **global**, **semantic** and **component** tokens: **global** tokens are specific of the theme and are to be tweaked globally, **semantic** and **component** tokens are used in code and implements **global** tokens.

The rule of thumb is to avoid **global** tokens in the components code.


#### Example
##### **Global** tokens
```css
--blue-500: ...
--spacing-4: ...
```

##### **Semantic** tokens
**For dark/light themes**: never invert global tokens. Instead, redefine semantic tokens inside a media query or a `.dark` class.

```css
/* Light mode (Default) */
:root {
  --bg-surface: var(--gray-100);
  --text-primary: var(--gray-900);
  --border-subtle: var(--gray-300);
}

/* Dark mode */
@media (prefers-color-scheme: dark) {
  :root {
    --bg-surface: var(--gray-900);
    --text-primary: var(--gray-100);
    --border-subtle: var(--gray-700);
  }
}

```


- must define **semantic** tokens specifically for interactions:
```css
--color-primary-hover: ;
--color-primary-active: ;
```


- must define focus rings for keyboard-focused elements
```css
--color-focus-ring: ;
```



##### **Component** tokens
```css
--button-bg: var(--color-primary);
```