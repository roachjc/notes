---
title: "Async Rendering"
lesson: 1
chapter: 3
date: "01/04/2018"
category: "tech"
type: "lesson"
tags:
    - react
    - react-16
---

Async rendering is a core performance feature of React fiber but has not yet been implemented. It prevents react from blocking the main thread as it renders large updates / components.

Introducing async rendering can remove control over the order in which components are rendered which may not be desirable.

* the *render phase* is when React compares the updated tree with existing tree and calculates any changes that have occurred. This work is done asynchronously and can be interupted / paused etc.

* the *commit phase* is when changes are painted to the screen. This is done synchronously.

The intention is to expose an API that provides the developer control over when to begin the commit phase. So you'd be able to hold off rendering component A until component B is also ready.

```javascript
// This is the root of the react tree, attached to the DOM
const root = ReactDOM.creatRoot(domElement);

// Render the App asynchronously. preRender is an async function...
const asyncRenderedApp = root.preRender(<App />)

// ... so we can do some other stuff here
// and then await its completion
await asyncRenderApp;
// before entering the commit phase
asyncRenderedApp.commit();
```