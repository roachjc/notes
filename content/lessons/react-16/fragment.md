---
title: "Fragment"
lesson: 3
chapter: 3
date: "05/01/2018"
category: "tech"
type: "lesson"
tags:
    - React
---

Fragments prevent the need to group child components / elements within a div or span. They therefore remove the need for these superfluous DOM nodes to be created and appended.

In React 16.0 it was possible to wrap children in an array instead of a DOM node. This is no longer the preferred method.

React now provides a Fragment component on the React object. One could also reference `<React.Fragment> </React.Fragment>` directly. The Fragment component is able to accept a key attribute for when mapping a collection of Fragments.

```javascript
import React, { Fragment } from 'react';

render() {
  return (
    <Fragment>
      Some text.
      <h2>A heading</h2>
      More text.
      <h2>Another heading</h2>
      Even more text.
    </Fragment>
  );
}
```
Syntactic support is now added as follows. This syntax cannot accept attributes
```javascript
render() {
  return (
    <>
      Some text.
      <h2>A heading</h2>
      More text.
      <h2>Another heading</h2>
      Even more text.
    </>
  );
}
```
Requires Babel 7+ for shorthand
