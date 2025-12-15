
# 1. About
### [Building blocks of a web application](https://nextjs.org/learn/react-foundations/what-is-react-and-nextjs#building-blocks-of-a-web-application)

There are a few things you need to consider when building modern applications. Such as:

- **User Interface** - how users will consume and interact with your application.
- **Routing** - how users navigate between different parts of your application.
- **Data Fetching** - where your data lives and how to get it.
- **Rendering** - when and where you render static or dynamic content.
- **Integrations** - what third-party services you use (for CMS, auth, payments, etc.) and how you connect to them.
- **Infrastructure** - where you deploy, store, and run your application code (serverless, CDN, edge, etc.).
- **Performance** - how to optimize your application for end-users.
- **Scalability** - how your application adapts as your team, data, and traffic grow.
- **Developer Experience** - your team's experience building and maintaining your application.

For each part of your application, you will need to decide whether you will build a solution yourself or use other tools, such as packages, libraries, and frameworks.

**imperative programming** is like giving a chef step-by-step instructions on how to make a pizza. **Declarative programming** is like ordering a pizza without being concerned about the steps it takes to make the pizza. 🍕

[React](https://react.dev/) is a popular declarative library that you can use build user interfaces.

# JSX

### rules of jsx:
https://react.dev/learn/writing-markup-with-jsx#the-rules-of-jsx

JSX is a syntax extension for JavaScript that allows you to describe your UI in a familiar _HTML-like_ syntax.

Browsers don't understand JSX out of the box, so you'll need a JavaScript compiler, such as a [Babel](https://babeljs.io/), to transform your JSX code into regular JavaScript.


# [React core concepts](https://nextjs.org/learn/react-foundations/building-ui-with-components#react-core-concepts)

There are three core concepts of React that you'll need to be familiar with to start building React applications. These are:

- ## Components
	User interfaces can be broken down into smaller building blocks called **components**.
	https://react.dev/learn/your-first-component
- ## Props
	Similar to a JavaScript function, you can design components that accept custom arguments (or props) that change the component's behavior or what is visibly shown when it's rendered to the screen. Then, you can pass down these props from parent components to child components.
	**Note:** In React, data flows down the component tree. This is referred to as _one-way data flow_. State, which will be discussed in the next chapter, can be passed from parent to child components as props.
	https://nextjs.org/learn/react-foundations/displaying-data-with-props
	https://react.dev/learn/passing-props-to-a-component
	https://react.dev/learn/conditional-rendering
- ## State
	Add `useState()` to your project. It returns an array, and you can access and use those array values inside your component using **array destructuring**:
	The first item in the array is the state `value`, which you can name anything. It's recommended to name it something descriptive:
	The second item in the array is a function to `update` the value. You can name the update function anything, but it's common to prefix it with `set` followed by the name of the state variable you're updating:
	You can also take the opportunity to add the initial value of your `likes` state to `0`:
	```js
	function HomePage() {
	  // ...
	  const [likes, setLikes] = React.useState(0);
	}
	```
	**Note**: Unlike props which are passed to components as the first function parameter, the state is initiated and stored within a component. You can pass the state information to children components as props, but the logic for updating the state should be kept within the component where state was initially created.'
	[Adding Interactivity](https://react.dev/learn/adding-interactivity)
	[Managing State](https://react.dev/learn/managing-state)
	[State: A component's memory](https://react.dev/learn/state-a-components-memory)
	[Meet your first hook](https://react.dev/learn/state-a-components-memory#meet-your-first-hook)
	[Responding to Events](https://react.dev/learn/responding-to-events)

- [Next.js Routing Fundamentals](https://nextjs.org/docs/app/building-your-application/routing)
- [Defining Routes](https://nextjs.org/docs/app/building-your-application/routing/defining-routes)
- [Pages and Layouts](https://nextjs.org/docs/app/building-your-application/routing/pages-and-layouts)

- [Server Components Docs](https://nextjs.org/docs/app/building-your-application/rendering/server-components)
- [Client Component Docs](https://nextjs.org/docs/app/building-your-application/rendering/client-components)
- [Composition Patterns](https://nextjs.org/docs/app/building-your-application/rendering/composition-patterns)
- [The "use client" Directive](https://react.dev/reference/react/use-client%3E)
- [The "use server" Directive](https://react.dev/reference/react/use-server)

# Some Page Optimization
## Other Methods You Can Use to Fine-Tune for High Performance

While your React tab component is functional and animated, there are opportunities for further optimization to ensure high performance, especially in scenarios involving larger datasets or asynchronous data fetching. Let's explore some strategies to fine-tune your tab component.

### Data Considerations

- Size of the Tab Component: Check the size of your tab component, especially if dealing with a large amount of data. Consider lazy-loading or pagination to improve initial rendering times.
- Asynchronous Data Loading: If your data is asynchronous, implement loading states to enhance user experience. Optimize the rendering logic to handle data loading efficiently.

### React Profiler

- Identifying and Reducing Unnecessary Re-renders: Outside leveraging React DevTools Profiler to analyze component rendering behaviour, use React hooks like React.memo and React.callback to optimize and cache your data.

### Component Structure

- Granular Component Structure: For larger tab components, consider breaking down the structure into smaller, granular components. This allows for better maintainability and can improve rendering performance.

### Code Splitting

- Dynamic Imports for Code Splitting: Implement dynamic imports for code splitting, especially if your React application becomes more complex. This ensures that only the necessary code loads when a specific tab is selected.

### Responsive Design

- Optimizing for Different Viewports: Ensure your tab component is responsive to different screen sizes. Consider media queries and adaptive design to provide an optimal experience across various devices.

### Browser Caching

- Optimizing Assets with Browser Caching: Leverage browser caching for static assets, such as images, to reduce load times. Consider optimizing and compressing images to cut their impact on performance.

### Testing and Profiling

- Continuous Testing and Profiling: Test your tab component under various conditions and profiles. Use tools like Lighthouse or Web Vitals to track performance metrics and address any issues that may arise.