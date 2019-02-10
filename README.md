## Create the development environment
### Create package.json with basic initialization properties:
```
$ npm init
```

### Create .gitignore file in the project root directory:
```
$ touch .gitignore
```

### Add these directories & files for Git to ignore:
```
.DS_STORE
node_modules
build
```

### Add React and React-Dom dependencies:
```
$ npm install react@15.5.4 react-dom@15.5.4 --save
```

### Add webpack module bundler development dependency:
```
$ npm install webpack@3.4.0 --save-dev
```

### Install Webpack globally for access to Webpack commands in the Terminal:
```
$ npm install webpack@3.4.0 -g
```

### Create webpack.config.js file in the project root directory:
```
$ touch webpack.config.js
```

### Add the basic configuration to webpack.config.js:
```
const webpack = require('webpack');
const { resolve } = require('path');

module.exports = {

  entry: [
    resolve(__dirname, "src") + "/index.jsx"
  ],

  output: {
    filename: 'app.bundle.js',
    path: resolve(__dirname, 'build'),
  },

  resolve: {
    extensions: ['.js', '.jsx']
  }

};
```

### Add Babel dependencies:
#### Babel Core - primary library
```
$ npm install babel-core@6.24.1 --save-dev
```

#### Babel loader - allows webpack to load Babel
```
$ npm install babel-loader@7.0.0 --save-dev
```

#### Babel ES5 preset - allows transpilation to ES5
```
$ npm install babel-preset-es2015@6.24.1 
```

#### Babel React preset - allows Babel to transpile React code
```
$ npm install babel-preset-react@6.24.1 --save-dev
```

### Instruct Webpack to use Babel loader to pre-process code before bundling (a.k.a transformation):
```
  module: {
    rules: [
      {
        test: /\.jsx?$/,
        loader: "babel-loader",
        exclude: /node_modules/,
        options: {
          presets: [
            "es2015",
            "react"
          ]
        }
      },
    ],
  }
  ```
- **test** takes a RegEx expression indicating which files the loader should transform.
- **loader** details which loader tool will transform these files
- **exclude** outlines which files, if any, should not be transformed. We don't need to transform our npm dependencies so we list node_modules here.
- **options** tells Babel what kind of project we’re working with (React), and what version of JavaScript code should be transpiled to (ES5).

### Add Prop Types to dependencies:
```
$ npm install prop-types@15.5.10 --save
```

### Install Webpack Development server globally:
```
$ npm install webpack-dev-server@2.5.0 -g
```

### Install Webpack Development server in the project (locally):
```
$ npm install webpack-dev-server@2.5.0 --save-dev
```

### Allow npm to import React libraries - add import statements to the top of src/index.jsx:
```
import React from "react";
import ReactDOM from "react-dom";
```

### Install React Hot Loader package:
```
$ npm install react-hot-loader@3.0.0-beta.7 --save-dev
```

### Update webpack.config.js to use hot loading (see "<-- new code" in the following code block):
```
const webpack = require('webpack');
const { resolve } = require('path');

module.exports = {

  entry: [
    'react-hot-loader/patch', <-- new code
    'webpack-dev-server/client?http://localhost:8080', <-- new code
    'webpack/hot/only-dev-server', <-- new code
    resolve(__dirname, "src", "index.jsx")
  ],

  output: {
    filename: 'app.bundle.js',
    path: resolve(__dirname, 'build'),
    publicPath: '/'
  },

  resolve: {
    extensions: ['.js', '.jsx']
  },

  devtool: '#source-map', <-- new code

  devServer: { <-- new code
    hot: true,
    contentBase: resolve(__dirname, 'build'),
    publicPath: '/'
  },

  module: {
    rules: [
      {
        test: /\.jsx?$/,
        loader: "babel-loader",
        exclude: /node_modules/,
        options: {
          presets: [
            ["es2015", {"modules": false}], <-- new code
            "react",
          ],
          plugins: [
            "react-hot-loader/babel" <-- new code
          ]
        }
      }
    ]
  },

  plugins: [ <-- new code
    new webpack.HotModuleReplacementPlugin(),
    new webpack.NamedModulesPlugin(),
  ]
};
```

### Update index.js:
```
import React from 'react';
import ReactDOM from 'react-dom';
import App from './components/App';
import { AppContainer } from 'react-hot-loader'; <-- new code

const render = (Component) => {
  ReactDOM.render(
    <AppContainer> <-- new code
      <Component/>
    </AppContainer>,
    document.getElementById('react-app-root')
  );
};

render(App);

if (module.hot) { <-- new code
  module.hot.accept('./components/App', () => {
    render(App)
  });
}
```

### Install HTML-Webpack-Plugin dependency:
```
$ npm install html-webpack-plugin@2.29.0 --save-dev
```
- Programmatically generates an index.html file using Webpack

### Create an Embedded JS template:
```
touch template.ejs
```

### Add content to the template:
```
<!DOCTYPE html>
<head>
  <meta charset="utf-8">
  <title><%= htmlWebpackPlugin.options.title %></title>
</head>
  <body>
    <% if (htmlWebpackPlugin.options.appMountId) { %>
      <div id="<%= htmlWebpackPlugin.options.appMountId%>"></div>
    <% } %>
  </body>
</html>
```
This is the template that Webpack will use to create an index.html file when bundling code.

### Bundle code with Webpack:
```
$ webpack
```

### Start the Webpack Development server:
```
$ webpack-dev-server
```

- The project will be available at http://localhost:8080

## Begin building components
### Create components directory:
```
$ mkdir src/components
```

### Create App component - components are Capitalized:
```
$ touch src/components/App.jsx
```

### Add App component blueprint to App.jsx:
```
import React from "react";

function App(){
  return (
    <div>
      <h1>Help Queue</h1>
      <h3>3a</h3>
      <h3>Thato and Haley</h3>
      <p><em>Firebase entries not saving!</em></p>
      <hr/>
    </div>
  );
}

export default App;
```

### Set up the entry point, index.jsx, to use the App component:
```
import React from "react";
import ReactDOM from "react-dom";
import App from "./components/App";

ReactDOM.render(
  <App/>,
  document.getElementById('react-app-root')
);
```

#### The blueprint
##### App component file
- Always import React from the "react" npm package at the top of the file.

- Most components are functions. (Class-based components should be used very sparingly.)

- Component function names begin with a capital letter and match the component filename.

- The function returns the JSX this component will render in the browser.

##### index.jsx import App statement
- Components reside in their own file, and are exported as a module so other areas of the application may import it.

## Creating Nested components
### Create Header component:
```
$ touch src/components/Header.jsx
```

### Add boilerplate to Header.jsx:
```
import React from "react";

function Header(){
  return (
    <h1>Help Queue</h1>
  );
}

export default Header;
```

### Import Header into App.jsx:
```
import React from "react";
import Header from "./Header";
```

### Render the Header in App:
```
import React from "react";
import Header from "./Header";

function App(){
  return (
    <div>
      <Header/>
      <h3>3a</h3>
      <h3>Thato and Haley</h3>
      <p><em>Firebase will not save record!</em></p>
    </div>
  );
}

export default App;
```

### Create TicketList component:
``` 
touch src/components/TicketList.jsx
```

### TicketList boilerplate with JSX for Ticket component:
```
import React from "react";
import Ticket from "./Ticket";

function TicketList(){
  return (
    <Ticket/>
  );
}

export default TicketList;
```

### Create Ticket component:
``` 
touch src/components/Ticket.jsx
```

### Ticket boilerplate plus Ticket JSX:
```
import React from "react";

function Ticket(){
  return (
    <div>
      <h3>3a</h3>
      <h3>Thato and Haley</h3>
      <p><em>Firebase will not save record!</em></p>
    </div>
  );
}

export default Ticket;
```

### Update App.jsx imports
```
import React from "react";
import Header from "./Header";
import TicketList from "./TicketList";

function App(){
  return (
    <div>
      <Header/>
      <TicketList/>
    </div>
  );
}

export default App;
```

### Import PropTypes dependency into Ticket.jsx:
```
import React from "react";
import PropTypes from "prop-types";

function Ticket(props){
  return (
      <div>
        <h3>{props.location} - {props.names}</h3>
        <p><em>{props.issue}</em></p>
        <hr/>
      </div>
  );
}

Ticket.propTypes = {
  names: PropTypes.string.isRequired,
  location: PropTypes.string.isRequired,
  issue: PropTypes.string
};

export default Ticket;
```