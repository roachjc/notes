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

// Combine reducers