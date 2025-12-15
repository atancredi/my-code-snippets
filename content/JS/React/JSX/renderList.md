# Render a component for every entry in a list
```jsx
{links.map((link) => (
	<Link title={link.title} url={link.url} key={link.id}></Link>
))}
```