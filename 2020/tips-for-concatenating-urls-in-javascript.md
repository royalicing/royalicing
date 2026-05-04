---
date: 2020-10-01
---
<meta property="og:published_time" content="2020-10-01">

# Tips for concatenating URLs in JavaScript

You might have a base URL from configuration, that you then concatenate with paths to produce a full URL. Say an API URL you fetch from.

But should you provide the base URL with or without a trailing slash? e.g. Should you have `https://api.example.org/` or `https://api.example.org`?

If you simply combine the strings together, you can end up with double slashes:

```js
const baseURL = 'https://api.example.org/';

const productsURL = baseURL + '/products';
// 'https://api.example.org//products'
```

We could try and check for the double slash, but that’s a pain:

```js
const productsURL = baseURL +
  (baseURL.endsWith('/') ? 'products' : '/products');
```

Wouldn’t you just rather not worry about this? Turns out we can with the built-in [`URL` class](https://developer.mozilla.org/en-US/docs/Web/API/URL). It’s supported in [all modern browsers (not IE)](https://caniuse.com/url).

```js
const baseURL = 'https://api.example.org/';

const productsURL = new URL('/products', baseURL).toString();
// 'https://api.example.org/products'
```

Isn’t that better? It’s also available in [Node.js](https://nodejs.org/api/url.html#url_the_whatwg_url_api) and [Deno](https://doc.deno.land/deno/stable/~/URL), so we can use the same technique everywhere.
