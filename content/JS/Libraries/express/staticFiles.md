```javascript
app.use('/static', express.static('public'))
```

```javascript
app.use(express.static('public'))
```

```javascript
const path = require('path')
app.use('/static', express.static(path.join(__dirname, 'public')))
```


[Fonte](https://expressjs.com/en/starter/static-files.html)
