---
title: "Server-Side Rendering"
lesson: 4
chapter: 3
date: "08/01/2018"
category: "tech"
type: "lesson"
tags:
    - React
---

#### ReactDOMServer
Using react server remains the same.
```javascript
import { renderToString } from 'react-dom/server';
import MyPage from "./MyPage"

// Using Express
app.get("/", (req, res) => {
  res.write("<!DOCTYPE html><html><head><title>My Page</title></head><body>");
  res.write("<div id='content'>");
  // Transforms react tree into initial HTML
  res.write(renderToString(<MyPage/>));
  res.write("</div></body></html>");
  res.end();
});
```
#### hydrate()

```javascript
import { hydrate } from "react-dom"
import MyPage from "./MyPage"

hydrate(<MyPage/>, document.getElementById("content"));
```
Using ReactDOM.render to hydrate server rendered string is deprecated, to be removed v17. ReactDOM.Hydrate allows return of arrays, strings and numbers from components to be SSR'd. For example, this would not be possible using `render`:
```javascript
class myComponent extends Component {
  render() {
    return 'a string not a react element'
    // OR
    return [
      <div>an</div>,
      <div>array</div>,
      <div>of divs</div>
    ]
    // OR return a number
    return 1;
  }
}
```
☝️ *do not confuse the reactDOM render method with the react component render method* ☝️

#### Efficiencies
The HTML strings generated in renderToString are now significantly smaller as many react specific DOM elements and comments have been removed.

v16 performs less strict comparison to check equality between client and server-side markup and only amends subtree on server when mismatch is found rather that rewriting whole tree as was previously the case.

Note that it is now especially important to address any mismatch warnings appearing in dev mode as v16 will not fix mismatched HTML attributes.

v16 now only checks `process.env.NODE_ENV === 'production'` once. It's an expensive operation and previously it was being checked all over the place!

#### Streaming
Instead of rendering to string it is now possible to render directly to a node stream. Not 100% on the mechanics of this - is it an HTTP2 multiplexing feature? - but allows for the document to be streamed to the browser before following parts have been generated. Improves TTFB.

```javascript
// using Express
import { renderToNodeStream } from "react-dom/server"
import MyPage from "./MyPage"

app.get("/", (req, res) => {
  res.write("<!DOCTYPE html><html><head><title>My Page</title></head><body>");
  res.write("<div id='content'>"); 
  
  // create stream
  const stream = renderToNodeStream(<MyPage/>);
  
  // pipe the stream to the response object
  // options.end.false allows us to append the end of the html
  stream.pipe(res, { end: false });
  
  // append end of html on completion of stream and end response
  stream.on('end', () => {
    res.write("</div></body></html>");
    res.end();
  });
});
```
Note that this will not work when wrapped in renderToStaticMarkup. EG:
```javascript
res.write(renderToStaticMarkup(
  <html>
    <head>
      <title>My Page</title>
    </head>
    <body>
      <div id="content">
        { renderToString(<MyPage/>) }
      </div>
    </body>
  </html>);
  ```
  Nor will it work where the render pass adds anything to the HTML before the main SSR chunk (eg in the head of the document).

#### Limitations
React 16 does not support [portals](/portals) or [error boundaries](error-boundaries) on the server.

