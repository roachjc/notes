---
title: "Reducers"
lesson: 2
chapter: 2
date: "04/01/2018"
category: "tech"
type: "lesson"
tags:
    - React
    - Redux
---

A reducer accepts the previous state and the action being dispatched.

A reducer returns the next state of the application.

A reducer **does not** modify the state given to it. It takes the state and creates a new state with any modifications dictated by the action incorporated.

There is always a only a single function that takes the whole state of the application and any action and returns a new whole state of the application.

#### Composing Reducers
It's possible to compose a reducer using nested reducers. For example, this reducer handles the whole list of todos but passes off the concern of managing a single todo to another reducer function

```javascript
const todos = (state = [], action) => {
  switch (action.type) {
    case 'ADD_TODO':
      return [
        ...state,
        todo(undefined, action),
      ];
    case 'TOGGLE_TODO':
      return state.map(t =>
        todo(t, action)
      );
    default:
      return state;
  }
};
```
This second function that deals only with a todo (not a list of todos) is passed a single todo and the action. In the case of `ADD_TODO` the todo it is passed is undefined. For `TOGGLE_TODO` it is passed every todo in the list via `map`.
```javascript
const todo = (state, action) => {
  switch (action.type) {
    case 'ADD_TODO':
      return {
        id: action.id,
        text: action.text,
        completed: false,
      };
    case 'TOGGLE_TODO':
      if (state.id !== action.id) {
        return state;
      }
      return {
        ...state,
        completed: !state.completed,
      };
    default:
      return state;
  }
};
```
Note that in `TOGGLE_TODO` if the id does not match that in the action, it does nothing and just returns the todo unchanged. Otherwise we spread the todo object into a new returned object and overwrite the completed property with its opposite.

#### Combining Reducers

Redux provides a utility function to [combine reducers](https://redux.js.org/docs/api/combineReducers.html). This allows for separation of reducer concerns in our files while continuing to provide a single reducer to pass to the store.

```javascript
import { combineReducers } from 'redux';

const rootReducer = combineReducer({ reducerOne, reducerTwo , reducerN})
```
The combineReducers function accepts an object of reducers. It returns a single reducer that can either be passed to createStore or combined again using combineReducers.

The required single 'root' reducer is passed to the the `createStore` redux method. 
