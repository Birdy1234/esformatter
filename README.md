# esformatter

[![Build Status](https://secure.travis-ci.org/millermedeiros/esformatter.png?branch=master)](https://travis-ci.org/millermedeiros/esformatter)

ECMAScript code beautifier/formatter.



## Why?

[jsbeautifier.org](http://jsbeautifier.org/) doesn't have enough options and
not all IDEs/Editors have a good JavaScript code formatter. I would like to
have a command line tool (and standalone lib) as powerful/flexible as the
[WebStorm](http://www.jetbrains.com/webstorm/) and
[FDT](http://fdt.powerflasher.com/) code formatters.

This tool could also be reused by other node.js libs like
[escodegen](https://github.com/Constellation/escodegen/) to format the output
(so each lib has a single responsibility).

For more reasoning behind it and history of the project see: [esformatter
& rocambole](http://blog.millermedeiros.com/esformatter-rocambole/)



## How?

This tool uses [rocambole](https://github.com/millermedeiros/rocambole) (based
on Esprima) to recursively parse the tokens and transform it *in place*.



## Goals

 - granular control about white spaces, indent and line breaks.
 - have as many settings as possible so the user can tweak it to his own needs.
 - command line interface (cli).
 - be non-destructive.
 - option to control automatic semicolon insertion (asi).
 - support for local/global config file so settings can be shared between team
   members.
 - be the best JavaScript code formatter.



## Important

This tool is still on early development and is missing support for many
important features.

Contributors are always welcome.


## API

### esformatter.format(str[, opts]):String

So far `esformatter` exposes a single `format()` method which receives a string
containing the code that you would like to format and the configuration options
that you would like to use and returns a string with the result.

```js
var esformatter = require('esformatter');

// for a list of available options check "lib/preset/default.json"
var options = {
    // inherit from the default preset
    preset : 'default',
    indent : {
        value : '  '
    },
    lineBreak : {
        before : {
            BlockStatement : 1,
            DoWhileStatementOpeningBrace : 1,
            // ...
        }
    },
    whiteSpace : {
        // ...
    }
};

var fs = require('fs');
var codeStr = fs.readFileSync('path/to/js/file.js', 'utf-8');

// return a string with the formatted code
var formattedCode = esformatter.format(codeStr, options);
```

### CLI

You can also use the simple CLI to process `stdin` and `stdout`:

```sh
# format "test.js" and output result to stdout
cat test.js | esformatter
# process "test.js" and writes to "test.out.js"
esformatter < test.js > test.out.js
```

CLI will be highly improved after we complete the basic features of the
library. We plan to add support for local/global settings and some flags to
toggle the behavior.



## Project structure / Contributing

We will create `-wip` branches (work in progress) for *unfinished* features
(mostly because of failing tests) and try to keep master only with *stable*
code. We will try hard to not rewrite the commit history of `master` branch but
will do it for `-wip` branches.

If you plan to implement a new feature check the existing branches, I will push
all my local `-wip` branches if I don't complete the feature in the same day.
So that should give a good idea on what I'm currently working.

Try to split your pull requests into small chunks (separate features), that way
it is easier to review and merge. But feel free to do large refactors as well,
will be harder to merge but we can work it out.

The easiest way to add new features and fix bugs is to create a test file with
mixed input and use the [esprima parser
demo](http://esprima.org/demo/parse.html) to visualize the syntax tree and
implement each step separately.



### Default Settings

The default settings should be as *conservative* as possible, [Google
JavaScript Style
Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
should be used as a reference.


### Tests

Tests are done by comparing the result of `esformatter.parse()` of files with
name ending on `-in.js` with the files `-out.js`. The folder
`test/compare/default` tests the default settings and files inside
`test/compare/custom` tests custom settings. Tests inside the `compare/custom`
folder should try to test the *opposite* of the default settings whenever
possible.

To run the tests install the devDependencies by running `npm install --dev`
(only required once) and then run `npm test`.

`mocha` and `expect.js` source code was edited to provide better error
messages. See [mocha/issues/657](https://github.com/visionmedia/mocha/pull/657)
and [expect.js/issues/34](https://github.com/LearnBoost/expect.js/pull/34) for
more info.

To check code coverage run `npm test --coverage`.




## Popular Alternatives

 - [jsbeautifier](http://jsbeautifier.org/)



## License

Released under the MIT license


