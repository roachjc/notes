---
title: "Portals"
lesson: 3
chapter: 3
date: "05/01/2018"
category: "tech"
type: "lesson"
tags:
    - React
---

Portals allow appending react elements to nodes other than the single DOM node defined in `ReactDOM.render`.

This is useful for components such as dialogs, modals, tooltips etc.

Let's say we have two DOM nodes in our JS. We are going to append our app to one as normal. We'll append a single component to the other.

```javascript
// These two containers are siblings in the DOM
const appRoot = document.getElementById('app-root');
const modalRoot = document.getElementById('modal-root');
```
The Modal component will be hidden by default. Visibility is set via state and conditionally rendering Modal or null dependent on state value
```javascript
// This component will be mounted onto the modalRoot
class Modal extends React.Component {
  constructor(props) {
    super(props);
    // Create a div that we'll render the modal into.
    this.el = document.createElement('div');
  }

  componentDidMount() {
    // Append the element into the DOM on mount.
    modalRoot.appendChild(this.el);
  }

  componentWillUnmount() {
    // Remove the element from the DOM when we unmount
    modalRoot.removeChild(this.el);
  }
  
  render() {
    // Use a portal to render the children into the element
    return ReactDOM.createPortal(
      // Any valid React child: JSX, strings, arrays, etc.
      this.props.children,
      // A DOM element
      this.el,
    );
  }
}
```
The Modal component is a normal React component, so we can
render it wherever we like without needing to know that it's
implemented with portals.

```javascript
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {showModal: false};
  }

  toggleModal = () => {
    this.setState({showModal: !this.state.showModal});
  }

  render() {
    // Conditionally render component
    const modal = this.state.showModal ? (
      <Modal>
        <div className="modal">
          <div>
            With a portal, we can render content into a different
            part of the DOM, as if it were any other React child.
          </div>
          This is being rendered inside the #modal-container div.
          <button onClick={this.toggleModal}>Hide modal</button>
        </div>
      </Modal>
    ) : null;

    return (
      <div className="app">
        This div has overflow: hidden.
        <button onClick={this.toggleModal}>Show modal</button>
        {modal}
      </div>
    );
  }
}
```
The App is rendered to the appRoot as normal. Despite being a child of the App in React, the modal is appended to modalRoot on the DOM.
```javascript
ReactDOM.render(<App />, appRoot);
```
Note that the position of the portal component remains unchanged inside the react tree. It therefore works in exactly the same way. Its context is unchanged and has access to state and children in the same way.

As a result, events fired in a Portal will propagate through the **React tree** if uncaught - not the DOM tree.
