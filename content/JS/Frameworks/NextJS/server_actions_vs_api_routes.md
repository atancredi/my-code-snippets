API routes are highly customizable endpoints that can support all the HTTP verbs and respond with any kind of payload. The downside of APIs is that they aren't inherintely type-safe.
Server Actions, on the other hand, are type-safe out of the box when you use them within the context of the NextJS application. The issue with server actions is that you don't get as much control over the format of the payload.

https://www.pronextjs.dev/should-i-use-server-actions-or-apis
---

Server actions seems to just be better, so are there any use cases for having API routes in the app directory anymore?
1. NextAuth
2. You want to have an api you can call outside of the Next.js app
3. Server actions are in alpha / not yet stable (still true as of 2024?sobbit)
4. Server actions cannot be used to communicate with a third party application. Say you have a Wordpress site or a React Native app that needs to leverage the NextJS backend, you would need API routes.
5. The react primitives that transport data through server actions are not yet implemented. So if you want to update data from an action you currently need to flush the components with a ‘router.refresh()’ or leverage API routes for client side data fetching after the action is finished.

_server actions from my perspective are not a good idea in terms of code design, reusability and scalability._
If the main reason you are using Next is because you need an API server, you shouldn’t need server actions. But I think we can all agree this is not the general use case of the Next framework. Most Next apps are full stack react applications that get great benefit from the server actions paradigm.
So if you need to support both a Next App and a Native app, you would use both server actions, and an api route that wraps those server actions. If you only have a Next App you would just use server actions, and if you just had Native App you would just use API routes.

**for talking to third party applications is preferred to use API routes**

https://www.reddit.com/r/nextjs/comments/13hnl2b/app_directory_server_actions_vs_api_routes/