unassertify
================================

[Browserify](http://browserify.org/) transform to encourage reliable programming by writing assertions in production code, and compiling them away from release.

[![Build Status][travis-image]][travis-url]
[![NPM version][npm-image]][npm-url]
[![Dependency Status][depstat-image]][depstat-url]
[![License][license-image]][license-url]


#### RELATED MODULES

- [unassert](https://github.com/twada/unassert): Encourage reliable programming by writing assertions in production code, and compiling them away from release.
- [babel-plugin-unassert](https://github.com/twada/babel-plugin-unassert): Babel plugin for unassert
- [webpack-unassert-loader](https://github.com/zoncoen/webpack-unassert-loader): Webpack loader for unassert
- [gulp-unassert](https://github.com/twada/gulp-unassert): Gulp plugin for unassert
- [unassert-cli](https://github.com/twada/unassert-cli): CLI for unassert


INSTALL
---------------------------------------

```
$ npm install --save-dev unassertify
```


HOW TO USE
---------------------------------------


### via CLI

```
$ $(npm bin)/browserify -t unassertify /path/to/src/target.js > /path/to/build/target.js
```

### via API

```javascript
var source = require('vinyl-source-stream');
var browserify = require('browserify');
var glob = require('glob'),

gulp.task('production_build', function() {
    var files = glob.sync('./src/*.js');
    var b = browserify({entries: files});
    b.transform('unassertify');
    return b.bundle()
        .pipe(source('bundle.js'))
        .pipe(gulp.dest('./dist'));
});
```


EXAMPLE
---------------------------------------

For given `math.js` below,

```javascript
'use strict';

var assert = require('assert');

function add (a, b) {
    console.assert(typeof a === 'number');
    assert(!isNaN(a));
    assert.equal(typeof b, 'number');
    assert.ok(!isNaN(b));
    return a + b;
}
```

Run `browserify` with `-t unassertify` to transform file.

```
$ $(npm bin)/browserify -t unassertify /path/to/demo/math.js > /path/to/build/math.js
```

You will see assert calls disappear.

```javascript
'use strict';
function add(a, b) {
    return a + b;
}
```


SUPPORTED PATTERNS
---------------------------------------

Assertion expressions are removed when they match patterns below. In other words, unassertify removes assertion calls that are compatible with Node.js standard [assert](https://nodejs.org/api/assert.html) API (and `console.assert`).

* `assert(value, [message])`
* `assert.ok(value, [message])`
* `assert.equal(actual, expected, [message])`
* `assert.notEqual(actual, expected, [message])`
* `assert.strictEqual(actual, expected, [message])`
* `assert.notStrictEqual(actual, expected, [message])`
* `assert.deepEqual(actual, expected, [message])`
* `assert.notDeepEqual(actual, expected, [message])`
* `assert.deepStrictEqual(actual, expected, [message])`
* `assert.notDeepStrictEqual(actual, expected, [message])`
* `assert.fail(actual, expected, message, operator)`
* `assert.throws(block, [error], [message])`
* `assert.doesNotThrow(block, [message])`
* `assert.ifError(value)`
* `console.assert(value, [message])`

unassertify also removes assert variable declarations,

* `var assert = require("assert")`
* `var assert = require("power-assert")`

and assignments.

* `assert = require("assert")`
* `assert = require("power-assert")`


AUTHOR
---------------------------------------
* [Takuto Wada](https://github.com/twada)


LICENSE
---------------------------------------
Licensed under the [MIT](http://twada.mit-license.org/2015-2016) license.


[npm-url]: https://npmjs.org/package/unassertify
[npm-image]: https://badge.fury.io/js/unassertify.svg

[travis-url]: https://travis-ci.org/twada/unassertify
[travis-image]: https://secure.travis-ci.org/twada/unassertify.svg?branch=master

[depstat-url]: https://gemnasium.com/twada/unassertify
[depstat-image]: https://gemnasium.com/twada/unassertify.svg

[license-url]: http://twada.mit-license.org/2015-2016
[license-image]: https://img.shields.io/badge/license-MIT-brightgreen.svg
