# karma-webpack-with-fast-source-maps

**This is a fork of [karma-webpack](https://github.com/webpack/karma-webpack) which enables file source maps and hot testing**

## Installation

``` sh
npm install --save-dev karma-webpack-with-fast-source-maps
```

## Usage

``` javascript
    // in karma.conf.js

		files: [
			// only specify one entry point
			// and require all tests in there
			'test/test_index.js'
		],

		preprocessors: {
			// add webpack as preprocessor
			'test/test_index.js': ['webpack']
		},
```

``` javascript
// test/test_index.js

// This gets replaced by karma webpack with the updated files on rebuild
var __karmaWebpackManifest__ = [];

// require all modules ending in "_test" from the
// current directory and all subdirectories
var testsContext = require.context(".", true, /_test$/);

function inManifest(path) {
  return __karmaWebpackManifest__.indexOf(path) >= 0;
}

var runnable = testsContext.keys().filter(inManifest);

// Run all tests if we didn't find any changes
if (!runnable.length) {
  runnable = testsContext.keys();
}

runnable.forEach(testsContext);
```

Every test file is required using the [require.context](http://webpack.github.io/docs/context.html#require-context) and compiled with webpack into one test bundle.
When a file changes, only the affected tests will be run.
If a failure occurs, the failing group will be rerun each run until they pass.
If no tests are affected by a change, all tests are rerun (if you `touch` your test_index.js it will run all tests).

## Source Maps

### File (Faster)

File source maps are faster and work out of the box. Use:

``` javascript
webpack: {
  // ...
	devtool: 'cheap-module-source-map'
}
```

### Inline

Or, if you want inline source maps, you can use the `karma-sourcemap-loader`:

```
npm install --save-dev karma-sourcemap-loader
```

And then add it to your preprocessors

``` javascript
preprocessors: {
	'test/test_index.js': ['webpack', 'sourcemap']
}
```

And tell webpack to generate sourcemaps

``` javascript
webpack: {
  // ...
	devtool: 'inline-source-map'
}
```

## Options

This is the full list of options you can specify in your Karma config.

### webpack

Webpack configuration.

### webpackMiddleware

Configuration for webpack-dev-middleware.

## License

Copyright 2014-2015 Tobias Koppers

[MIT](http://www.opensource.org/licenses/mit-license.php)
