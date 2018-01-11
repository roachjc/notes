---
title: "Async Await"
lesson: 1
chapter: 4
date: "01/04/2018"
category: "tech"
type: "lesson"
tags:
    - async
    - promises
    - es6
---

Although introduced in Node 7.6, Node 8 provides LTS

#### Async Await refresher

* The `await` keyword can only be used in an `async` function.
* The `await`ed function must return a promise.
* Use `try catch` to handle errors.

```javascript
const getStuff = async () => {
  try {
    // pause execution and await an asynchronous task
    const response = await JSON.parse(fetch(DATA));
    
    // Use conditionals inside async function
    if (isIncomplete(response)) {

      // getMore is some other function that returns a promise
      const more = await getMore();
      return more;
    } else {

      // return value is a promise
      return response;
    }

  // Handle errors here
  } catch (err) {
      doSomethingWith(err);
  };
}

// The async function returns a promise that resolves to return val of async func
getStuff().then(eitherResponseOrMore => doSomethingWith(eitherResponseOrMore));
```

#### Using in Express

```javascript
app.get('users/:id', async (req, res, next) => {
  try {
    const user = await getUser({ id: req.params.id });
    res.json(user);
  } catch (err) {
    handle(err);
    next();
  }
}) 
```