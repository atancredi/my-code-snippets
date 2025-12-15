# Data structure polyfills

## check if variable is array
```js
function isArray(arr) {
    if (typeof (arr) == "object" && Array.isArray(arr)) return true;
    else return false;
}
```

## check if variable is object
```js
function isObject(arr) {
    if (typeof (arr) == "object" && !(Array.isArray(arr))) return true;
    else return false;
}
```


# Formatting strings and numbers
```js
function format(string, array_parametri) {
    var counter = 0;
    var stringa_out = stringa;

    try {
        //check if the array is an array
        if (!isArray(array_parametri))
            throw 'The parameters array is not an array!';

        //execute
        array_parametri.forEach(function (item) {
            if (typeof (item) == "string" || typeof item == "number") {
                var expr = "{" + counter + "}";
                if (stringa.includes(expr)) {
                    stringa_out = stringa_out.replaceAll(expr, item);
                } else {
                    throw 'Not allowed string';
                }
            } else {
                throw 'The parameter is not the right type';
            }

            counter++;
        });
    } catch (e) {
        console.error(e)
        return;
    }

    return stringa_out;

}
```

```js
//format a number (ordini di grandezza)
function formatNumber(num) {
    if (num > 999 && num < 1000000) { return (num / 1000).toFixed(1) + 'K'; }
    else if (num > 1000000) { return (num / 1000000).toFixed(1) + 'M'; }
    else if (num < 900) { return num; }
}
```

# jQuery polyfills
## screen resolution
```js
function getScreenRes() {
    return $(window).width() + "x" + $(window).height()
}
```

## add styles to a jQuery object
```js
function addStyles(obj, styles) {
    console.log(obj, styles)
    for (var i = 0; i < lenght(Object.keys(styles)); i++) {
        obj.css(Object.keys(styles)[i], styles[i]);
    }
}
```