---
title: "Promisify"
lesson: 3
chapter: 4
date: "01/10/2018"
category: "tech"
type: "lesson"
tags:
    - async
    - node
---

Promisify converts a callback-based node function into a Promise-based one. It is available on the Util object.
```javascript
const { promisify } - require('util');
```
It accepts a function with the regular node convention for callbacks, where it accepts a callback as its last argument. That callback accepts `(err, value)` as its args. For example.
```javascript
fs.readFile(path, (err, data) => doSomethingWith(data));
```
To convert this to return a promise:
```javascript
const fs = require('fs');
const { promisify } = require('util');

const readFile = promisify(fs.readFile);

const getFile = async (pathToFile) => {
  try {
    const file = await readFile(pathToFile);
    doSomethingWith(file);
  } catch (err) {
    doSomethingWith(err);
  }
}

getFile('./path/to/file.ext').then(() => crackOn());
``` 