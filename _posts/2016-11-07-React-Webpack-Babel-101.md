---
layout: post
title: React, Webpack, Babel 101 (in progress!)
---

# Table of Contents
  * [Chapter 1 - React, Webpack, Babel 101](#chapter-1)

## Chapter 1 - React, Webpack, Babel 101 <a id="chapter-1"></a>

* [Reference React.js, Webpack, Babel](https://www.codementor.io/tamizhvendan/beginner-guide-setup-reactjs-environment-npm-babel-6-webpack-du107r9zr)

- Create project directory
`mkdir react-hello-world && cd react-hello-world`

- Create package.json for Dependency Management
`npm init`

- Configure Webpack (Task Automation) to generate static assets representing modules with dependencies that it bundles
`npm install --save webpack`

- Create webpack.config.js
`touch webpack.config.js`

{% highlight javascript %}
var webpack = require('webpack');
var path = require('path');

// Path of bundle file output
var BUILD_DIR = path.resolve(__dirname, 'src/client/public');

// Path of React.js codebase
var APP_DIR = path.resolve(__dirname, 'src/client/app');

var config = {

    // Starting point of app
    entry: APP_DIR + '/index.jsx',

    // Instructions for Webpack to output bundle.js after bundling process
    output: {
        path: BUILD_DIR,
        filename: 'bundle.js'
    }
};

module.exports = config;
{% endhighlight %}

- Create input subfolder
`mkdir src && mkdir src/client && mkdir src/client/app && touch src/client/app/index.jsx`

- Create sample JSX in src/client/app/index.jsx
`console.log('Hello');`

- Run Webpack to compile and generate bundle.js and bundle.js.map file in src/client/public using Development Mode 
`./node_modules/.bin/webpack -d`

- Open index.html in browser

- Change from `var config = {` to `module.exports = { debug: true, ...`

- Run Webpack in Dev Mode `... /webpack -d` or Prod Mode `... /webpack -p` (minified)
- See [CLI Wiki](https://github.com/webpack/docs/wiki/cli)

```
$ ./node_modules/.bin/webpack -d
Hash: 7d66afc04ec55432dfc0
Version: webpack 1.13.3
Time: 87ms
        Asset     Size  Chunks             Chunk Names
    bundle.js  1.66 kB       0  [emitted]  main
bundle.js.map  1.62 kB       0  [emitted]  main
   [0] ./src/client/app/index.jsx 27 bytes {0} [built]
```

- Create src/client/index.html to using bundle.js output

```
<html>
  <head>
    <meta charset="utf-8">
    <title>React.js using NPM, Babel6 and Webpack</title>
  </head>
  <body>
    <div id="app" />
    <script src="public/bundle.js" type="text/javascript"></script>
  </body>
</html>
```

- Add babel-loader to translate JSX and ES6 (to be supported by browsers) before Webpack bundling
`npm install babel-loader babel-core babel-preset-es2015 babel-preset-react --save`

- Configure babel-loader to use ES6 and JSX plugins (better than alternative to adding to webpack.conf.js `... , query: {presets: ['react', 'es2015']}`
`touch .babelrc`
```
{
  "presets" : ["es2015", "react"]
}
```

- Update Webpack configuration to use babel-loader before bundling files
```
...
var config = {
  ...
  module : {
    
    // Array of loader properties as elements (i.e. babel-loader, etc)
    loaders : [
      {
        // File extension (i.e. .js and .jsx) the loader processes via the test property
        test : /\.jsx?/,
        include : APP_DIR,
        loader : 'babel' // Name of the loader (i.e. babel-loader)
      }
    ]
  }
}
```

- Install React and React DOM
`npm install react react-dom --save`

- Change index.jsx to:
```
import React from 'react';
import {render} from 'react-dom';

class App extends React.Component {
  render () {
    return <p> Hello React!</p>;
  }
}

render(<App/>, document.getElementById('app'));
```

- Run Webpack again
`./node_modules/.bin/webpack -d`

- Open index.html in browser

- Customise Webpack with Watch to automatically re-bundle when change detected. Reload webpage still required
`./node_modules/.bin/webpack -d --watch`

- Configure tool runners using NPM by updating package.json with the following for Development and Production Modes:
```
  "scripts": {
    "dev": "webpack -d --watch",
    "build" : "webpack -p"
  },
```

- Now just run with `npm run dev` or `npm run build`

- Add engines to package.json

```
  "engines": {
    "node": "5.10.1",
    "npm": "3.10.3"
  },
```  

- Add `publicPath` to webpack.prod.config.js to explicitly state directory containing bundle outputs so not get error public/bundle.js not found error
- References: https://github.com/webpack/docs/wiki/Configuration

```
 publicPath: "/src/client/"
```

- Install Express server `npm install express --save`

- Create Procfile for Heroku i.e. `web: npm run webpack_prod; npm run express-server;` or use `postinstall:` in package.json to process before server starts instead 

- Add [concurrently](https://codingbox.io/react-for-beginners-part-1-setting-up-repository-babel-express-web-server-webpack-a3a90cc05d1e#.clvjirdpa)

`npm install concurrently --save-dev`

```
  "scripts": {
    "webpack_dev_watch": "./node_modules/.bin/webpack -d --watch --config webpack.dev.config.js",
    "express-server": "node ./server",
    "dev": "concurrently --kill-others \"npm run webpack_dev_watch\" \"npm run express-server\"",
    "webpack_prod": "./node_modules/.bin/webpack -p --config webpack.prod.config.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
```

- Setup source maps by updating webpack.dev.config.js with below code then open Chrome Dev Tools and go to Sources > top > webpack:// > . > src/client/app / ; and add breakpoints to debug

```
    output: {
				...
        sourceMapFilename: 'bundle.js.map',
        devtoolModuleFilenameTemplate: 'webpack:///[resource-path]?[hash]',
        devtoolFallbackModuleFilenameTemplate: 'webpack:///[resourcePath]?[hash]'
    },

    devtool: 'source-map'
```

- Setup CSS and Style loaders for Webpack to inject style tag into DOM. Install with NPM to avoid errors when run i.e. `Module not found: Error: Cannot resolve module 'css'`

```
npm install style-loader --save
npm install css-loader --save
```

- Setup ENV in webpack.prod.config.js to prevent warning messages on Heroku saying `No ENV file found`
```
plugins: [
    new webpack.DefinePlugin({
        "process.env": {
            NODE_ENV: JSON.stringify("production"),
            PORT: 5000
        }
    })
],
```


- Test works local after removing dependencies `rm -rf node_modules` 

- Create Heroku app and use [heroku local](https://devcenter.heroku.com/articles/heroku-local) to test locally
```
heroku create reactvr
heroku local web
git add --all .
git commit
git push heroku master
heroku open
heroku restart
heroku logs --tail
heroku run bash
```

### React Definitions and Links

* Architecture

- [Facebook - Beginner - Thinking in React](https://facebook.github.io/react/docs/thinking-in-react.html)
- [Facebook - Advanced - Design Thinking](https://facebook.github.io/react/contributing/design-principles.html)

### Webpack Definitions and Links


* Webpack
- All files can be Modules and loaded (i.e. require("xyz.js") )
- Module bundler loads modules and generating large single output with low performance
- Module bundler configuration options to split into multiple output files (**chunks**) and load parts asynchronously (**on demand**) to improve performance 
- See [Webpack Tutorial](http://webpack.github.io/docs/tutorials/getting-started/)
- See [Webpack Wiki Contents](https://github.com/webpack/docs/wiki/contents)
- See [code splitting](https://github.com/webpack/docs/wiki/code-splitting)
- [Webpack Examples GitHub](https://github.com/webpack/webpack/tree/master/examples)
- [Comparison with Browserify](https://github.com/webpack/docs/wiki/comparison)
- [Webpack different Loaders i.e. CSS, Sass, ES6, Autoprefix](https://julienrenaux.fr/2015/03/30/introduction-to-webpack-with-practical-examples/)
- [Example webpack.config.js file - excellent](https://github.com/JedWatson/react-select/issues/176)
- [Example using different babel dependencies](http://ditrospecta.com/javascript/react/es6/webpack/heroku/2015/08/08/deploying-react-webpack-heroku.html)
- [Example using webpack-dev-server](http://www.christianalfoni.com/articles/2015_04_19_The-ultimate-webpack-setup)
- [Example sing webpack-hot-middleware](https://github.com/webpack/webpack/issues/2230)

* Example App
- [React with Webpack excellent tips and other links](https://medium.com/@rajaraodv/webpack-the-confusing-parts-58712f8fcad9#.e2yqenl2f)
- [React with Twilio used](https://www.twilio.com/blog/2015/11/reactjs-tutorial-call-monitoring-with-react-express-and-socket-io.html)


* Webpack Loaders
- [Webpack Loaders](http://webpack.github.io/docs/loaders.html)
- [html-loader](https://github.com/webpack/html-loader)
- Automatically generate index.html with correct location of Webpack bundle.js dependency

* ECMAScript 6
- Arrows, Classes, Generators, Modules, etc

* [Babel](http://babeljs.io/) Loader
- Webpack uses Babel Loader to translate JSX and ES6 (so they are supported by browsers) before bundling
- babel-loader translates JSX and ES6 using plugins including babel-preset-es2015 and babel-preset-react
- babelrc is Babel Loader configuration

* Webpack-dev-server
- Development builds using lightweight Express Node.js server on port 8080
- Internally calls Webpack
- Live Reload capability by changing just the changed module using Hot Module Replacement (HMR)

## TODO

* [Flux](https://facebook.github.io/flux/)
* [Alt](http://alt.js.org/)
* [Excellent React Setup Guide](https://codingbox.io/react-for-beginners-part-1-setting-up-repository-babel-express-web-server-webpack-a3a90cc05d1e#.clvjirdpa)
