---
title: fetch
description: How to get data using `fetch`
---


## About

- Browser
    - The `fetch` function is builtin for modern browsers.
- NPM
    - [node-fetch](https://www.npmjs.com/package/node-fetch)
        - The browser `fetch` function brought to Noded.js.
        - The docs show how to use it.
    - [fetch](https://www.npmjs.com/package/fetch)
        - Unrelated to `fetch` function but shown for interest.
- [JavaScript.info](https://javascript.info/fetch) site
- [Mozilla docs](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)

Using `fetch`. Builtin for modern browsers and you can use `node-fetch` on the server side.

Warning - an error will **not** be thrown on a non-200 status so you must handle this yourself. Other libraries tend to handle errors for.


## Samples

There are async and promise-based examples below. Both can be adapted to follow the other style.

### Async approach

Using the modern `async` and `await` syntax.

```javascript
const url = 'https://api.github.com/users/github'
const resp = await fetch(url);
```

Then either:

```javascript
// JSON such as from REST API or static file.
const json = await resp.json();
```

```javascript
// Plain text such as scraping HTML.
const body = await resp.text()
```

### With error handling

Use `resp.ok` to check for a success message.

Based on _JS.info_ example. I don't know if `alert` will actually break the flow or code after will still get executed.

```javascript
const resp = await fetch(url);

if (resp.ok) {
  let json = await resp.json();
} else {
  alert(`HTTP-Error: ${resp.status}`);
}
```

My own variation.

```javascript
const resp = await fetch(url);
if (!resp.ok) {
  throw new Error(`${resp.status} - ${resp.statusText}`);
}
const data = await resp.json();
```

### Promises approach

Based on Mozilla docs and `node-fetch` docs.

Warning - getting a non-2XX response will not trigger an error, so you need to add `resp.ok` check in your app.

```javascript
const url = 'https://api.github.com/users/github'

fetch(url)
  .then(resp => resp.json())
  .then(json => console.log(json);
  .catch(err => console.error(err));
```

### POST requests

Note that method is case-insensitive.

#### Send JSON data

```javascript
const body = { a: 1 };

const options = {
  method: 'POST',
  body:    JSON.stringify(body),
  headers: { 'Content-Type': 'application/json' },
}

await fetch('https://httpbin.org/post', options)
```

A more complex example.

```javascript
const query = new URLSearchParams(query).toString();
const url = `/myendpoint?${query}`;

const body = JSON.stringify(body);

const options = {
  method:  'POST',
  headers: { 'Content-Type': 'application/json' },
  body,
}

const resp = await fetch(
  url, 
  options
);
```

#### Send form data

```javascript
const body = 'a=1';

const options = { 
  method: 'POST', 
  body,  
}

await fetch(
  'https://httpbin.org/post', 
  options
)
```
