## How to install

Just run

```shell
npm install express-basic-auth
```

## How to use
https://www.npmjs.com/package/express-basic-auth#how-to-use

The module will export a function, that you can call with an options object to get the middleware:

```js
const app = require('express')()
const basicAuth = require('express-basic-auth')

app.use(basicAuth({
    users: { 'admin': 'supersecret' }
}))
```