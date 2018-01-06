---
title: "Store"
lesson: 3
chapter: 2
date: "04/01/2018"
category: "tech"
type: "lesson"
tags:
    - React
    - Redux
---

Create a store, typically in `configureStore.js` at or near root level of project

```javascript
import { createStore } from 'redux';
import rootReducer from './reducers';

const store = createStore(rootReducer);
```
Optionally you can pass in initial state, for example from server on load.
```javascript
const store = createStore(rootReducer, initialStateFromServer)
```
The returned store has a few methods:
```javascript
// return current state
store.getState();

// dispatch action
store.dispatch(action);

// subscribe
const unsubscribe = store.subscribe(() =>
  // callback runs every time the store updates
);
// unsubscribe with returned function
unsubscribe();
