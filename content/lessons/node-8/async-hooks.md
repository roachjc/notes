---
title: "Async Hooks"
lesson: 2
chapter: 4
date: "01/10/2018"
category: "tech"
type: "lesson"
tags:
    - async
    - node
---
*Currently Experimental*

Async Hooks allow users to track asynchronous resources created in node. It does this by providing an API for accessing four separate stages in an asynchronous event.
```javascript
const asyncHook = async_hooks.createHook({ init, before, after, destroy, [promiseResolve] });
```

Each of these four hooks is a callback is passed to `createHook` as a method in an options object.
#### init(asyncId, type, triggerAsyncId, resource)
Called during construction. The params passed in are basically what they sound like. `resource` is an object that represents the resource being monitored.
#### before(asyncId)
Called just before the callback of the monitored async operation is executed
#### after(asyncId)
Called just after the callback of the monitored async operation is executed
#### destroy(asyncId)
Called after the specified resource (async operation) is destroyed

Async hooks are disabled by default and must be enabled prior to calling the async function it is intended to monitor and should be disabled after that function has been called.
```javascript
asyncHook.enable();
someAsyncFunctionToMonitor();
asyncHook.disable();
```

[example](https://nodejs.org/api/async_hooks.html#async_hooks_asynchronous_context_example)