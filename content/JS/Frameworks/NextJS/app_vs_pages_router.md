# Extract
According to NextJs documentation, you can use both /app and /pages folders.
/app is kind of the updated version of the /pages folder, so at this point the /pages folder is supported as a way to migrate to the new /app folder
I would say that technically you can use both, but I think you shouldn't.
https://stackoverflow.com/questions/75976390/can-i-use-both-app-and-pages-folder-on-my-next-13-app
---
# Pros and cons
## Pages Directory
**Pros:**
- Easy to understand & utilize
- Built-in support for server-side rendering & React Server Components
- Good for small applications
- Simple & straightforward routing
- Good for static pages
- Automatic routing based on file structure
**Cons:**
- Limited flexibility and support for shared layouts and nested routing
- Hard to maintain for large applications, May require additional configuration for more complex routing needs
- Hard to share components between pages
- May not be suitable for all use cases
## App Directory
**Pros:**
- More flexible
- Supports Next.js 13 new features like layouts, error components, and loading components
- Better for large applications
- Leverages React’s server components for better performance
- Better for dynamic pages
- Provides more advanced routing and layout capabilities
- Better for sharing components between pages
- Offers a new way of building apps with welcomed improvements to architecture
**Cons:**
- More complex
- Experimental and not yet considered stable for production use
- Steep learning curve
- Some features, like nested layouts, [caching](https://medium.com/@CraftedX/why-i-hate-next-js-13-caching-4cd5a31d3a78) may still have limitations & problems
- Requires more setup and may not be necessary for simpler applications
## how to choose
- Use the Pages directory if you want simple and straightforward routing, automatic routing based on file structure, and easy creation of pages as React components.
- Use the App directory if you need more advanced routing and layout capabilities, want to leverage React’s server components for better performance, and want to customize your app’s behavior or layout.
- Consider using both directories if you want to incrementally adopt the App directory while still using the Pages directory for simpler pages.
- Keep in mind that the App directory is experimental and not yet considered stable for production use, and may require additional learning and understanding of the new paradigm.
https://medium.com/@CraftedX/should-you-use-next-js-pages-or-app-directory-38e803fe5cb4
---
# Finally
it's common amongst users to still prefer the pages router because it's simpler and has broader compatibility
https://www.reddit.com/r/nextjs/comments/17rq3ug/why_do_you_use_pages_instead_of_app_router/