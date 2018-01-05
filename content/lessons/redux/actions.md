---
title: "Actions"
lesson: 1
chapter: 2
date: "04/01/2018"
category: "tech"
type: "lesson"
tags:
    - React
    - Redux
---

Actions are plain Javascript objects. They describe in the simplest form, what changes should happen to the state.

The *only* way to modify state is to dispatch an action

Typically we list them as named exports in actions/index.js

```javascript
export const requestTodos = (filter) => ({
  type: 'REQUEST_TODOS',
  filter,
});

export const addTodo = (text) => ({
  type: 'ADD_TODO',
  id: createRandomID(),
  text,
});

export const toggleTodo = (id) => ({
  type: 'TOGGLE_TODO',
  id,
});
```

When working asynchronously it becomes a little more complicated. But most of the complexity exists outside of  actions, in modifying the dispatch method to accept a promise, that this function will return prior to resolution.

```javascript
// Asynchronous action creator.
// Returns a promise, that resolves to the action object created by receiveTodos

export const fetchTodos = (filter) =>

  // returns a call to the async api method
  api.fetchTodos(filter).then(response =>

    // When resolved its response is transformed into an action via receiveTodos
    // But we need to have redux accept promises, because that's the initial return value here
    // SEE CONFIGURESTORE, where we amend store.dispatch().

    receiveTodos(filter, response)
  );
  ```

  Refer to the notes on the store and middleware for further info on dealing with async actions in Redux