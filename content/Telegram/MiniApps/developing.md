# Developing a Telegram MiniApp


## Importing the SDK
```jsx
<Script onLoad={() => { setLoaded(true) }} src='https://telegram.org/js/telegram-web-app.js?56' ></Script>
```
this must always be imported in every page, but it is strongly adviced not to use both sdk methods and directly using Telegram.WebApp methods (it can lead to bugs).