# Query string parameters

```js
// METODO CONSIGLIATO
const params = new Proxy(new URLSearchParams(window.location.search), {
  get: (searchParams, prop) => searchParams.get(prop),
});
// Get the value of "some_key" in eg "https://example.com/?some_key=some_value"
let value = params.some_key; // "some_value"

// SECONDO METODO
const urlSearchParams = new URLSearchParams(window.location.search);
const params = Object.fromEntries(urlSearchParams.entries());

// TERZO METODO
const urlParams = new URLSearchParams(window.location.search);
const myParam = urlParams.get('myParam');

// PURE JAVASCRIPT
function getParameterByName(name, url = window.location.href) {
    name = name.replace(/[\[\]]/g, '\\$&');
    var regex = new RegExp('[?&]' + name + '(=([^&#]*)|&|#|$)'),
        results = regex.exec(url);
    if (!results) return null;
    if (!results[2]) return '';
    return decodeURIComponent(results[2].replace(/\+/g, ' '));
}
// usage --- query string: ?foo=lorem&bar=&baz
var foo = getParameterByName('foo'); // "lorem"
var bar = getParameterByName('bar'); // "" (present with empty value)
var baz = getParameterByName('baz'); // "" (present with no value)
var qux = getParameterByName('qux'); // null (absent)

// NOTE: If a parameter is present several times (?foo=lorem&foo=ipsum), you will get the first value (lorem). There is no standard about this and usages vary, see for example this question: Authoritative position of duplicate HTTP GET query keys.

// NOTE: The function is case-sensitive. If you prefer case-insensitive parameter name, add 'i' modifier to RegExp

// NOTE: If you're getting a no-useless-escape eslint error, you can replace name = name.replace(/[\[\]]/g, '\\$&'); with name = name.replace(/[[\]]/g, '\\$&').
```