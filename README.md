# example-above-the-fold-hapijs

## <a name="hapijs-server"></a>Hapijs Server
* Let's use the [hapi-react-redux-starter] repo to scaffold our app.
* Create a hapi app using the following commands:

```bash
git clone https://github.com/electrode-samples/hapi-react-redux-starter.git hapiApp
cd hapiApp
npm install
```

* Run using the following:
```bash
NODE_ENV=development npm test
NODE_ENV=development npm start
```

## <a name="above-the-fold-only-server-render"></a>Above The Fold Only Server Render

[above-the-fold-only-server-render] is a React component for optionally skipping server side rendering of components 
outside above-the-fold (or inside of the viewport). This component helps render your components on the server that are 
above the fold and the remaining components on the client.

[above-the-fold-only-server-render] helps increase performance both by decreasing the load on renderToString and sending
 the end user a smaller amount of markup.

By default, the [above-the-fold-only-server-render] component is an exercise in simplicity; it does nothing and only 
returns the child component.

### Install

* Add the `above-the-fold-only-server-render` component:

```bash
npm install above-the-fold-only-server-render --save
```

* Add the `hapiApp/source/components/above-the-fold/above-the-fold.js` file: 

```jsx
import React from "react";
import { AboveTheFoldOnlyServerRender } from "above-the-fold-only-server-render";

export default class AboveFold extends React.Component {

  render() {
    return (
      <AboveTheFoldOnlyServerRender skip={true}>
        <div className="renderMessage" style={{color: "blue"}}>
          <h3>Above-the-fold-only-server-render: Increase Your Performance</h3>
          <p>This will skip server rendering if the 'AboveTheFoldOnlyServerRender'
            lines are present, or uncommented out.</p>
          <p>This will be rendered on the server and visible if the 'AboveTheFoldOnlyServerRender'
            lines are commented out.</p>
          <p>Try manually toggling this component to see it in action</p>
          <p>
            <a href="https://github.com/electrode-io/above-the-fold-only-server-render"
               target="_blank">Read more about this module and see our live demo
            </a>
          </p>
        </div>
      </AboveTheFoldOnlyServerRender>
    );
  }
}
```

* Add the `above-the-fold` route in `hapiApp/source/routes.js`: 

```jsx
import AboveFold from "./components/above-the-fold/above-the-fold";

<Route path="above-the-fold" component={AboveFold} />
```

* You can tell the component to skip server side rendering either by passing a `prop` `skip={true}` or by setting up `skipServerRender` in your app context and passing the component a `contextKey` `prop`.

* Let's explore passing `skip prop`; there is an example in
`<your-hapi-app>/components/above-fold-simple.jsx`. On the Home page, click the link to render the `localhost:3000/above-the-fold` page.

* The best way to demo this existing component is actually going to be in your `node_modules.`

* Navigate to `<your-hapi-app>/node_modules/above-the-fold-only-server-render/lib/components/above-the-fold-only-server-render.js` line 29:

```javascript
var SHOW_TIMEOUT = 50;
```

* When we use this module at [WalmartLabs](www.walmartlabs.com), it's all about optimization. You are going to change line 29 to slow down the SHOW_TIMEOUT so you can see the component wrapper in action:
Change this to:

```javascript
var SHOW_TIMEOUT = 3000;
```

* Run the commands below and test it out in your app:

```bash
NODE_ENV=development npm start
```

The code in the `<h3>` tags that are above and below the `<AboveTheFoldOnlyServerRender skip={true}> </AboveTheFoldOnlyServerRender>` will render first:

```javascript
  import React from "react";
  import {AboveTheFoldOnlyServerRender} from "above-the-fold-only-server-render";

  export class AboveFold extends React.Component {

    render() {
      return (
        <div>
          <h3>Above-the-fold-only-server-render: Increase Your Performance</h3>
          <AboveTheFoldOnlyServerRender skip={true}>
              <div className="renderMessage" style={{color: "blue"}}>
                <p>This will skip server rendering if the 'AboveTheFoldOnlyServerRender'
                  lines are present, or uncommented out.</p>
                <p>This will be rendered on the server and visible if the 'AboveTheFoldOnlyServerRender'
                  lines are commented out.</p>
                <p>Try manually toggling this component to see it in action</p>
                <p>
                  <a href="https://github.com/electrode-io/above-the-fold-only-server-render"
                    target="_blank">Read more about this module and see our live demo
                  </a>
                </p>
              </div>
          </AboveTheFoldOnlyServerRender>
          <h3>This is below the 'Above the fold closing tag'</h3>
        </div>
      );
    }
  }
```

You can also skip server side rendering by `setting context in your app and passing a contextKey prop`. Here is an example:

```js
  const YourComponent = () => {
      return (
        <AboveTheFoldOnlyServerRender contextKey="aboveTheFoldOnlyServerRender.SomeComponent">
          <div>This will not be server side rendered based on the context.</div>
        </AboveTheFoldOnlyServerRender>
      );
  };

  class YourApp extends React.Component {
    getChildContext() {
      return {
        aboveTheFoldOnlyServerRender: {
          YourComponent: true
        }
      };
    }

    render() {
      return (
        <YourComponent />
      );
    }
  }

  YourApp.childContextTypes = {
    aboveTheFoldOnlyServerRender: React.PropTypes.shape({
      AnotherComponent: React.PropTypes.bool
    })
  };
```

[hapi-react-redux-starter]: https://github.com/electrode-samples/hapi-react-redux-starter
[Above-the-fold-only-server-render]: https://github.com/electrode-io/above-the-fold-only-server-render