# AMD injection loader for webpack

[![Join the chat at https://gitter.im/aaronjensen/amd-inject-loader](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/aaronjensen/amd-inject-loader?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

## What Is It

This is a simple [webpack](http://webpack.github.io/) loader that allows you to override any dependency requested at the top of your code file in a normal `define` function block. It is currently very limited in scope. It doesn't handle nested `require` statements, and it doesn't overwrite or shim the `require` method itself.

If you are looking for something that works with CommonJS code, try the [`inject-loader`](https://github.com/plasticine/inject-loader).We tried that loader first, but it didn't work with AMD code and didn't work with our code formatting style, so we whipped this up to meet our immediate testing needs. The API is similar, but you don't need to specify ahead of time which dependencies you want to inject or leave alone. Simply pass in whatever injections you want each time you call the factory method. It can even change between tests in the same file.

## Usage

If working with a code file that looks like this:

**myfile.js**
```js
define( [ "lodash", "customApi" ], function () {
	return {
		init: function () {
			customApi.run()
		}
	}
});
```
You could inject a different `customApi` like this:

**myfile.spec.js**
```js
var myfileInjector = require( "amd-inject!./myfile" );
var myfile = myfileInjector({
	customApi: {
		run: function () {
			console.log( "A new run definition" );
		}
	}
});

myfile.init(); // Outputs A new run definition

```

## Options

To customize options (currently only one option is supported), add a `amdInjectLoader` key to your webpack config file:

```js
{
	loaders: [ ... ],
	...
	amdInjectLoader: {
		istanbul: true
	}
}
```

* **`istanbul`** - `true` or `false` (Defaults to `false`) – Include istanbul ignore blocks on the lines that handle the injection


[Documentation: Using loaders](http://webpack.github.io/docs/using-loaders.html)

## License

MIT (<http://www.opensource.org/licenses/mit-license.php>)
