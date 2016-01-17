# Reactuate

Reactuate is an opinionated stack for building React-based frontend applications. Core objectives are as follows:

1. Fork-and-forget (or fork-and-follow) is not a great way to keep up with what's happening in the frontend space. Therefore, starting off from a cloned boilerplate "kit" is not an acceptable solution. Reactuateis distributed as a __dependency__.
2. Reactuate is strictly a frontend-oriented stack and is not meant to provide a foundation for so called "universal" (server and client side) JavaScript applications.
3. Discovering and learning the capabilities of comprehensive stack solution could be complicated. What's even worse, maintaining such stacks can be equally painful. Therefore, unconventionally, Reactuate is written as a *literate program* and is meant to be read as a small book and be a concise reference manual — while being a functional library.
4. Don't cling on the legacy stuff. Move forward.

## Version

Current published version:

    0.1.0

## License

Reactuate is licensed under the terms of [Apache 2.0 license](LICENSE.md).

# Requirements

Reactuate requires the minimum of the following version of Node.js:

[Node.js version](#)

    v5.4.1

More recent versions are allowed, within boundary the major version:

[](# "|eval")

```js
var required = "_"Requirements: Node.js version"".split(/[v\.]/)
var version = process.version.split(/[v\.]/)
if (version[1] !== required[1] ||
    parseInt(version[2]) < parseInt(required[2]) ||
    (version[2] === required[2] && parseInt(version[3]) < parseInt(required[3]))) {
  console.log("Node.js _"Requirements: Node.js version" or higher is required, within v" + required[1])
  process.exit(1)
}
```

In order to produce the extracted source code of Reactuate (should you need it),
you will need `literate-programming` npm package to be installed. Currently required version:

[literate-programming version](#)

    0.8.4

# Getting started

As it has been mentioned before, Reactuate is distributed as a dependency, and can, therefore, be installed with npm (assuming you already initialized your
project with `npm init`):

```shell
$ npm install --save-dev reactuate
```

The rest of this manual will introduce you to concepts and software used in the
stack.

# Webpack Layer

We serve the application in development mode and package the production version
by employing [webpack](http://webpack.github.io). Current required version:

[webpack version](#)

    1.12.11

[Dependencies](#)

```json
"webpack": "_"Webpack Layer: webpack version"",
"webpack-dev-server": "_"Running a development Webpack Server: webpack-dev-server version"",
"html-webpack-plugin": "_"Webpack Configuration: html-webpack-plugin version""
```

## Webpack Configuration

There's normally little to no Webpack configuration tweaking you need to do.

By default, it will assume your index.html file in your project to be

[index file](#)

    index.html

It will process this file with `html-webpack-plugin`, version:

[html-webpack-plugin version](#)

    1.7.0

Here's a sample of how this file might look like

[index.html sample](# "html")
```html
<!doctype html>
<html lang="en">
	<head>
		<meta charset="utf-8">
  	<meta name="viewport" content="width=device-width, initial-scale=1">
		<title>Your application</title>
	</head>
	<body>
		<!-- The app hooks into this div -->
		<div id="app"></div>
		<!-- Due to the way webpack works, a lot of magic happens in this file. HtmlWebpackPlugin automatically includes all assets (e.g. bundle.js, main.css) with the correct HTML tags, which is why they are missing in this HTML file. Check out webpackconfig.js if you want to know more. -->
	</body>
</html>
```

It will search for your source code files in this directory in your project:

[source directory](#)

    src

It will assume your main file in that directory to be

[main file](#)

    index.js

It will produce builds into this directory in your project:

[build directory](#)

    build

[](#)

This is how the configuration is composed:

[webpack-config.js](# "save: | jstidy")
```js
var path = require('path')

var webpack = require('webpack');
var HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = function(options) {
  var plugins = []
  var production = process.env['NODE_ENV'] === 'production'
  var src = '_":source directory"'
  var main = '_":main file"'
  var index = '_":index file"'

  var entry = path.resolve(__dirname, path.join(src, main))
  if (!production) {
    entry = [
        "webpack-dev-server/client?http://localhost:3000", // Needed for hot reloading
        "webpack/hot/only-dev-server", // See above
        entry
      ]
  }

  if (!production) {
    plugins.push(new webpack.HotModuleReplacementPlugin())
    plugins.push(new HtmlWebpackPlugin({template: path.resolve(process.cwd(), index), inject: true}))
  }

  return {
    entry: entry,
    plugins: plugins,
    output: {
      path: path.resolve(process.cwd(), '_":build directory"'),
      publicPath: '/',
      filename: 'js/bundle.js'
    }
  }
}
```

## Running a development Webpack server

It is imperative to be able to run an easy to update, hot-reloadable development version of the application before shipping an optimized version of it. This is what `webpack-dev-server` does. Reactuate currently requires the following version:

[webpack-dev-server version](#)

    1.14.1

In order to start a development Webpack server, you can run:

[](#)
```shell
$ node node_modules/reactuate/webpack-dev-server.js
```

Alternatively, you can add a convenience helper to your `package.json`:

[](#)
```json
"scripts": {
  "start": "node node_modules/reactuate/webpack-dev-server.js"
}
```

With this you can simply run the following to start your development server:

[](#)
```shell
$ npm start
```

### Development server

[webpack-dev-server.js](# "save:")
```js
var path = require('path'),
    webpack = require('webpack'),
    WebpackDevServer = require('webpack-dev-server'),
    config = require(path.join(__dirname, 'webpack-config'))()

console.log('Starting server...\n')

new WebpackDevServer(webpack(config), { // Start a server
  publicPath: config.output.publicPath,
  hot: true, // With hot reloading
  inline: false,
  historyApiFallback: true,
  quiet: true // Without logging
}).listen(3000, 'localhost', function (err, result) {
  if (err) {
    console.log(err)
  } else {
    console.log('Server started')
    console.log('Listening at localhost:3000')
  }
})
```

# Language Layer

## Babel Layer

Reactuate encourages the use of recent versions of Babel. [Babel](http://babeljs.io) is a transpiler from
future versions of ECMAScript to JavaScript you can run in the browser today.

Reactuate currently depends on the following version of Babel:

[babel version](#)

    6.4.0

Babel 6 is still fairly new and unfortunately, not all
tools support it well, but this should be less of a problem going forward.

[Dependencies](#)

```json
"babel-core": "_"Babel Layer: version""
```
# React Layer

Reactuate currently depends on the following version of React:

[react version](#)

    0.14.6


# Appendix A. Package file

[package.json](#Appendix A. Package file "save:")

```json
{
  "name": "reactuate",
  "version": "_"Version"",
  "description": "Reactuate is an opinionated React-based stack",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" &&   exit 1"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/reactuate/reactuate.git"
  },
  "keywords": [
    "react",
    "redux",
    "frontend"
  ],
  "author": "Yurii Rashkovskii <yrashk@gmail.com>",
  "license": "Apache-2.0",
  "engines": {"node": ">=_"Requirements: Node.js version" <6.0"},
  "dependencies": {
    "react": "_"React Layer: react version"",
    _"Babel Layer: Dependencies",
    _"Webpack Layer: Dependencies"
  },
  "bugs": {
    "url": "https://github.com/reactuate/reactuate/issues"
  },
  "homepage": "https://github.com/reactuate/reactuate#readme"
}
```
# Appendix B. .gitignore

Basically, we should ignore everything that's not this source code and the license:

[.gitignore](# "save:")

    *
    !README.md
    !LICENSE.md