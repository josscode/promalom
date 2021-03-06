# Promalom

[![Greenkeeper badge](https://badges.greenkeeper.io/rosswarren/promalom.svg)](https://greenkeeper.io/)

A very tiny promise util library, designed to work with native Promise functionality. https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Promise

Very small in file size, very specific in functionality.

This library is intended for the Node environment. It can work well in the browser, however makes no attempt to polyfill Promise.

[![npm version](https://img.shields.io/npm/v/promalom.svg?style=flat-square)](https://www.npmjs.com/package/promalom)

# Getting Started

Installing the package

```sh
npm install --save promalom
```

Importing the library

```js
// import the whole library
const P = require("promalom");

// explicitly requiring individual functions through destructuring
const { create, wait, series } = require("promalom");

// explicitly requiring individual functions from their source files
const series = require("promalom/src/series");
```

## Functions

### Series

Run the specified promise returning functions in series. Ensures the previous promise is resolved before starting the current.

```js
P.series([
  myPromiseReturningFunction,
  anotherPromiseReturningFunction,
  someOtherPromiseReturningFunction
]);
```

#### Upload files one at a time using series

Assuming `uploadFile` is a function that takes the filename and returns a promise that resolves once uploaded

```js
const fileNames = ["image1.jpg", "image2.jpg", "image3.jpg"];

P.series(
  fileNames.map(fileName => {
    return () => uploadFile(fileName);
  })
);
```

### Wait

Wait the specified time in milliseconds and then resolve. Wraps setTimeout.

```js
P.wait(20);
```

#### Call a promise returning function with timeout of 1 second

```js
Promise.race(P.wait(1000), promiseReturningFunction);
```

### Flush Promises

Flushes all pending promises in the JavaScript process queue. This is very useful in tests where you have Promises that are resolved immediately but you have no way to return or await them in your test.

```js
P.flushPromises();
```

### Promisify

Convert a callback returning function to return a promise. The callback must be the last parameter.
The callback is expected to follow the Node error first callback pattern, where the first parameter of the callback is `error`.

```js
const fs = require('fs');

const readFileP = P.promisify(fs.readFile);

readFileP('/etc/passwd').then(data => {
    ...
}).catch(error => {
    ...
});
```
