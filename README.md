# Promise Tool
A promised library of tools for Node.js and the browser.

## Install
`npm install promise-tool`

## API
- `PromiseTool.series` A given task will not be started until the preceding task completes.
	- `tasks` <Array> The array of functions to execute in series.
		- `task` <Function> A function which returns a promise.
			- `resolve(value)` <Any> A value which will be appended to the beginning of each task/function parameters.
	- `parameters` <Array> An Array of parameters.
		- `parameter` <Any> This parameter is made available to each task/function.

- `PromiseTool.setTimeout`
	- `delay` <Number> The number of milliseconds to wait before calling resolve.
	- `...args` <Any> Optional arguments to pass when the resolve is executed.


- `PromiseTool.setInterval`
	- `delay` <Number> The number of milliseconds to wait before calling resolve.
	- `method` <Function> A function that repeats on each interval. This function will fire upon each interval unless one of the following returns are implemented.
		- Return Value Actions
			- `result` <Error> Any valid JavaScript error type. Will fire the reject and pass the error.
			- `result` <Boolean> A boolean that calls resolve if true or reject if false.
			- `result` <Any> Any thing returned besides `null`, `undefined`, `false`, and a valid `Error` type will resolve with that return value as the first argument.
			- `result` <Null, Undefined> Both are ignored and will not trigger the resolve or reject.
	- `...args` <Any> Optional arguments to pass when the resolve is executed.


- `PromiseTool.setImmediate`
	- `...args` <Any> Optional arguments to pass when the resolve is executed.


- `PromiseTool.clearTimeout`
	- `timeout` <Timeout> A Timeout object as returned by setInterval().


- `PromiseTool.clearInterval`
	- `interval` <Interval> A Interval object as returned by setInterval().


- `PromiseTool.clearImmediate`
	- `immediate` <Immediate> An Immediate object as returned by setImmediate().


## Examples

### Series
```JavaScript
var PromiseTool = require('../index.js');

function a (text, one, two) {
	return new Promise (function (resolve) {
		setTimeout(function () {
			text = `a-${one}${two}`;
			return resolve(text);
		}, 900);
	});
}

function b (text, one, two) {
	return new Promise (function (resolve) {
		setTimeout(function () {
			text = `${text} b-${one}${two}`;
			return resolve(text);
		}, 600);
	});
}

PromiseTool.series([a, b], ['', 1, 2]).then(function (result) {
	console.log(result); // a-12 b-12
});
```

### Timers
```JavaScript
var PromiseTool = require('promise-timers');
var delay = 500;

PromiseTool.setTimeout(delay).then(function (args) {
	// this refers to timeout
	console.log(args);
	console.log('timeout done');
});

var i = 0;

function method () {
	// this refers to interval
	if (i > 5) {
		return true;
	} else {
		console.log(i);
		i++;
	}
};

 PromiseTool.setInterval(delay, method).then(function (args) {
	// this refers to interval
	console.log(args);
	console.log('interval done');
});

PromiseTool.setImmediate().then(function (args) {
	// this refers to immediate
   console.log(args);
   console.log('immediate done');
});

```