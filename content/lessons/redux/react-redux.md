---
title: "React-Redux"
lesson: 4
chapter: 2
date: "05/01/2018"
category: "tech"
type: "lesson"
tags:
    - React
    - Redux
---

[React-Redux](https://github.com/reactjs/react-redux/blob/master/docs/api.md#api) provides access to the store within a React application.

### Provider
The provider is an HOC that accepts the store and makes it available to descendant components. Typically we wrap the whole App with the Provider.

It would be possible also to pass the store around but that defeats one of the benefits of redux 🙄.
```javascript
import React from 'react'
import { render } from 'react-dom'
import { Provider } from 'react-redux'
import store from './configureStore'
import App from './components/App'

render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```
### Connect
The connect method is what gives a component access to the store. Typically one would use connect in a component that *contains* the component that needs access to the store to improve composability.

Connect returns a new connected component
```javascript
import { connect } from 'react-redux';
import { actioncreator } from '../actions';
import PresentationalComponent from './PresentationalComponent';

const mapStateToProps = (state) => ({
  requiredProp: state.requiredProp,
})

const mapDispatchToProps = (dispatch) => ({
  onUserInteraction: (something) => {
    dispatch(actionCreator(something));
  },
})

const ConnectedComponent = connect(
  mapStateToProps,
  mapDispatchToProps
)(presentationalComponent)

export default ConnectedComponent;

```
##### mapStateToProps
mapStateToProps defines what state to pass to the component. The component becomes subscribed to the store. It accepts the whole state should perform filtering / amendment to pass only the state required by the child component. It must return an object that becomes available as state to the wrapped component.

`ownProps` is optional and passes along existing props flowing through the regular react prop tree.
```javascript
const mapStateToProps = (state, [ownProps]) => ({
  requiredProp: state.requiredProp,
  ...ownProps,
})
```
##### mapDispatchToProps
MapDispatchToProps similarly returns an object. Each property is a method that calls an action creator and is used to interact with the store from a descendant component.

The mapDispatchToProps argument can be an object of functions (each assumed to be an action creator). Each function will be merged into props but wrapped with the dispatched method already, so can be called directly and without reference to `dispatch`.
```javascript
const mapDispatchToProps = {
  actionCreator1,
  actionCreator2,
}
```
It can also be a function. If so, it is passed the dispatch method as param1 and optionally any ownProps as param2.
```javascript
const mapDispatchToProps = (dispatch, [ownProps]) => ({
  onUserInteraction: (something) => {
    dispatch(actionCreator(something));
  },
  ...ownProps,
})
```
If mapDispatchToProps is not passed, the dispatch method will be injected into the wrapped components props anyway.

See these handy [examples](https://github.com/reactjs/react-redux/blob/master/docs/api.md#examples) from the docs