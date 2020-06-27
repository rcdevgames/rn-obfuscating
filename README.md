# rn-obfuscating

Obfuscate selected source files when building for React Native.

## Installation

    yarn add rn-obfuscating --dev

or

    npm install rn-obfuscating --save-dev

## Usage

### /metro.config.js

```diff
module.exports = {
  transformer: {
    getTransformOptions: async () => ({
      transform: {
        experimentalImportSupport: false,
        inlineRequires: false,
      },
    }),
    babelTransformerPath: require.resolve("./transformer")   // add here the transformer.js
  },
};
```

### /transformer.js optional obfuscating

```js
const obfuscatingTransformer = require("react-native-obfuscating-transformer");

const filter = (filename) => {
  return filename.startsWith("src");
};

module.exports = obfuscatingTransformer({
  // this configuration is based on https://github.com/javascript-obfuscator/javascript-obfuscator
  obfuscatorOptions: {


    compact: true,
    controlFlowFlattening: false,
    controlFlowFlatteningThreshold: 0.75,
    deadCodeInjection: false,
    deadCodeInjectionThreshold: 0.4,
    debugProtection: false,
    debugProtectionInterval: true,
    disableConsoleOutput: false,
    domainLock: [],
    identifierNamesGenerator: "mangled",
    identifiersDictionary: [],
    identifiersPrefix: "",
    inputFileName: "",
    log: true,
    renameGlobals: false,
    renameProperties: false,
    reservedNames: [],
    reservedStrings: [],
    rotateStringArray: true,
    seed: 0,
    selfDefending: true,
    shuffleStringArray: true,
    sourceMap: false,
    sourceMapBaseUrl: "",
    sourceMapFileName: "",
    sourceMapMode: "separate",
    splitStrings: false,
    splitStringsChunkLength: 10,
    stringArray: true,
    stringArrayEncoding: false,
    stringArrayThreshold: 0.75,
    target: "node",
    transformObjectKeys: true,
    unicodeEscapeSequence: false,
  },
  upstreamTransformer: require("metro-react-native-babel-transformer"),
  emitObfuscatedFiles: false,
  enableInDevelopment: true,
  filter,
  trace: true,
});
```

## Configuration

Options are:

### `upstreamTransformer: MetroTransformer`

Defines what the first pass of code transformation is. If you don't use a custom transformer already,
you don't need to set this option.

TypeScript example:

```diff
 const obfuscatingTransformer = require('rn-obfuscating')
+ const typescriptTransformer = require('react-native-typescript-transformer')

 module.exports = obfuscatingTransformer({
+  upstreamTransformer: typescriptTransformer
 })
```

#### Default value: `require('metro/src/transformer')`

### `filter: (filename: string, source: string) => boolean`

Returns true for any files that should be obfuscated and false for any files which should not be obfuscated.

By default, it obfuscates all files in `src/**/*`

### `obfuscatorOptions: ObfuscatorOptions`

**Warning** — Not all options are guaranteed to produce working code. In particular, `stringArray` definitely breaks builds.

See the [javascript-obfuscator docs](https://github.com/javascript-obfuscator/javascript-obfuscator) for more info about what each option does.

```ts
interface ObfuscatorOptions {
  compact?: boolean
  controlFlowFlattening?: boolean
  controlFlowFlatteningThreshold?: 0.75
  deadCodeInjection?: boolean
  deadCodeInjectionThreshold?: 0.4
  debugProtection?: boolean
  debugProtectionInterval?: boolean
  disableConsoleOutput?: boolean
  domainLock?: string[]
  identifierNamesGenerator?: "hexadecimal" | "mangled"
  log?: boolean
  renameGlobals?: boolean
  reservedNames?: string[]
  rotateStringArray?: true
  seed?: 0
  selfDefending?: boolean
  sourceMap?: boolean
  sourceMapBaseUrl?: string
  sourceMapFileName?: string
  sourceMapMode?: "separate" | "inline"
  stringArray?: boolean
  stringArrayEncoding?: boolean
  stringArrayThreshold?: 0.75
  target?: "browser" | "extension" | "node"
  unicodeEscapeSequence?: boolean
}
```

### `trace: boolean`

Iff true, prints a list of files being obfuscated

#### Default value: `false`

### `emitObfuscatedFiles: boolean`

Iff true, emits the obfuscated versions of files alongside their originals, for comparison.

#### Default value: `false`

### `enableInDevelopment: boolean`

Iff true, enables obfuscation in development mode.

#### Default value: `false`

## License

MIT




## JavaScript Obfuscator Options

Following options are available for the JS Obfuscator:

#### options:

```javascript
{
    compact: true,
    controlFlowFlattening: false,
    controlFlowFlatteningThreshold: 0.75,
    deadCodeInjection: false,
    deadCodeInjectionThreshold: 0.4,
    debugProtection: false,
    debugProtectionInterval: false,
    disableConsoleOutput: false,
    domainLock: [],
    identifierNamesGenerator: 'hexadecimal',
    identifiersDictionary: [],
    identifiersPrefix: '',
    inputFileName: '',
    log: false,
    renameGlobals: false,
    renameProperties: false,
    reservedNames: [],
    reservedStrings: [],
    rotateStringArray: true,
    seed: 0,
    selfDefending: false,
    shuffleStringArray: true,
    sourceMap: false,
    sourceMapBaseUrl: '',
    sourceMapFileName: '',
    sourceMapMode: 'separate',
    splitStrings: false,
    splitStringsChunkLength: 10,
    stringArray: true,
    stringArrayEncoding: false,
    stringArrayThreshold: 0.75,
    target: 'browser',
    transformObjectKeys: false,
    unicodeEscapeSequence: false
}
```

#### CLI options:
```sh
    -v, --version
    -h, --help

    -o, --output

    --compact <boolean>
    --config <string>
    --control-flow-flattening <boolean>
    --control-flow-flattening-threshold <number>
    --dead-code-injection <boolean>
    --dead-code-injection-threshold <number>
    --debug-protection <boolean>
    --debug-protection-interval <boolean>
    --disable-console-output <boolean>
    --domain-lock '<list>' (comma separated)
    --exclude '<list>' (comma separated)
    --identifier-names-generator <string> [dictionary, hexadecimal, mangled]
    --identifiers-dictionary '<list>' (comma separated)
    --identifiers-prefix <string>
    --log <boolean>
    --rename-globals <boolean>
    --rename-properties <boolean>
    --reserved-names '<list>' (comma separated)
    --reserved-strings '<list>' (comma separated)
    --rotate-string-array <boolean>
    --seed <string|number>
    --self-defending <boolean>
    --shuffle-string-array <boolean>
    --source-map <boolean>
    --source-map-base-url <string>
    --source-map-file-name <string>
    --source-map-mode <string> [inline, separate]
    --split-strings <boolean>
    --split-strings-chunk-length <number>
    --string-array <boolean>
    --string-array-encoding <boolean|string> [true, false, base64, rc4]
    --string-array-threshold <number>
    --target <string> [browser, browser-no-eval, node]
    --transform-object-keys <boolean>
    --unicode-escape-sequence <boolean>
```

### `compact`
Type: `boolean` Default: `true`

Compact code output on one line.

### `config`
Type: `string` Default: ``

Name of JS/JSON config file which contains obfuscator options. These will be overridden by options passed directly to CLI

### `controlFlowFlattening`
Type: `boolean` Default: `false`

##### :warning: This option greatly affects the performance up to 1.5x slower runtime speed. Use [`controlFlowFlatteningThreshold`](#controlflowflatteningthreshold) to set percentage of nodes that will affected by control flow flattening. 

Enables code control flow flattening. Control flow flattening is a structure transformation of the source code that hinders program comprehension.

Example:
```ts
// input
(function(){
    function foo () {
        return function () {
            var sum = 1 + 2;
            console.log(1);
            console.log(2);
            console.log(3);
            console.log(4);
            console.log(5);
            console.log(6);
        }
    }
    
    foo()();
})();

// output
(function () {
    function _0x3bfc5c() {
        return function () {
            var _0x3260a5 = {
                'WtABe': '4|0|6|5|3|2|1',
                'GokKo': function _0xf87260(_0x427a8e, _0x43354c) {
                    return _0x427a8e + _0x43354c;
                }
            };
            var _0x1ad4d6 = _0x3260a5['WtABe']['split']('|'), _0x1a7b12 = 0x0;
            while (!![]) {
                switch (_0x1ad4d6[_0x1a7b12++]) {
                case '0':
                    console['log'](0x1);
                    continue;
                case '1':
                    console['log'](0x6);
                    continue;
                case '2':
                    console['log'](0x5);
                    continue;
                case '3':
                    console['log'](0x4);
                    continue;
                case '4':
                    var _0x1f2f2f = _0x3260a5['GokKo'](0x1, 0x2);
                    continue;
                case '5':
                    console['log'](0x3);
                    continue;
                case '6':
                    console['log'](0x2);
                    continue;
                }
                break;
            }
        };
    }

	_0x3bfc5c()();
}());
```

### `controlFlowFlatteningThreshold`
Type: `number` Default: `0.75` Min: `0` Max: `1`

The probability that the [`controlFlowFlattening`](#controlflowflattening) transformation will be applied to any given node.

This setting is especially useful for large code size because large amounts of control flow transformations can slow down your code and increase code size.

`controlFlowFlatteningThreshold: 0` equals to `controlFlowFlattening: false`.

### `deadCodeInjection`
Type: `boolean` Default: `false`

##### :warning: Dramatically increases size of obfuscated code (up to 200%), use only if size of obfuscated code doesn't matter. Use [`deadCodeInjectionThreshold`](#deadcodeinjectionthreshold) to set percentage of nodes that will affected by dead code injection.
##### :warning: This option forcibly enables `stringArray` option.

With this option, random blocks of dead code will be added to the obfuscated code. 

Example:
```ts
// input
(function(){
    if (true) {
        var foo = function () {
            console.log('abc');
            console.log('cde');
            console.log('efg');
            console.log('hij');
        };
        
        var bar = function () {
            console.log('klm');
            console.log('nop');
            console.log('qrs');
        };
    
        var baz = function () {
            console.log('tuv');
            console.log('wxy');
            console.log('z');
        };
    
        foo();
        bar();
        baz();
    }
})();

// output
var _0x5024 = [
    'zaU',
    'log',
    'tuv',
    'wxy',
    'abc',
    'cde',
    'efg',
    'hij',
    'QhG',
    'TeI',
    'klm',
    'nop',
    'qrs',
    'bZd',
    'HMx'
];
var _0x4502 = function (_0x1254b1, _0x583689) {
    _0x1254b1 = _0x1254b1 - 0x0;
    var _0x529b49 = _0x5024[_0x1254b1];
    return _0x529b49;
};
(function () {
    if (!![]) {
        var _0x16c18d = function () {
            if (_0x4502('0x0') !== _0x4502('0x0')) {
                console[_0x4502('0x1')](_0x4502('0x2'));
                console[_0x4502('0x1')](_0x4502('0x3'));
                console[_0x4502('0x1')]('z');
            } else {
                console[_0x4502('0x1')](_0x4502('0x4'));
                console[_0x4502('0x1')](_0x4502('0x5'));
                console[_0x4502('0x1')](_0x4502('0x6'));
                console[_0x4502('0x1')](_0x4502('0x7'));
            }
        };
        var _0x1f7292 = function () {
            if (_0x4502('0x8') === _0x4502('0x9')) {
                console[_0x4502('0x1')](_0x4502('0xa'));
                console[_0x4502('0x1')](_0x4502('0xb'));
                console[_0x4502('0x1')](_0x4502('0xc'));
            } else {
                console[_0x4502('0x1')](_0x4502('0xa'));
                console[_0x4502('0x1')](_0x4502('0xb'));
                console[_0x4502('0x1')](_0x4502('0xc'));
            }
        };
        var _0x33b212 = function () {
            if (_0x4502('0xd') !== _0x4502('0xe')) {
                console[_0x4502('0x1')](_0x4502('0x2'));
                console[_0x4502('0x1')](_0x4502('0x3'));
                console[_0x4502('0x1')]('z');
            } else {
                console[_0x4502('0x1')](_0x4502('0x4'));
                console[_0x4502('0x1')](_0x4502('0x5'));
                console[_0x4502('0x1')](_0x4502('0x6'));
                console[_0x4502('0x1')](_0x4502('0x7'));
            }
        };
        _0x16c18d();
        _0x1f7292();
        _0x33b212();
    }
}());
```

### `deadCodeInjectionThreshold`
Type: `number` Default: `0.4` Min: `0` Max: `1`

Allows to set percentage of nodes that will affected by `deadCodeInjection`.

### `debugProtection`
Type: `boolean` Default: `false`

##### :warning: Can freeze your browser if you open the Developer Tools.

This option makes it almost impossible to use the `console` tab of the Developer Tools (both on WebKit-based and Mozilla Firefox).

* WebKit-based: blocks the site window, but you still can navigate through Developer Tools panel.
* Firefox: does *not* block the site window, but still won't let you use DevTools.

### `debugProtectionInterval`
Type: `boolean` Default: `false`

##### :warning: Can freeze your browser! Use at own risk.

If checked, an interval is used to force the debug mode on the Console tab, making it harder to use other features of the Developer Tools. Works if [`debugProtection`](#debugprotection) is enabled.

### `disableConsoleOutput`
Type: `boolean` Default: `false`

Disables the use of `console.log`, `console.info`, `console.error`, `console.warn`, `console.debug`, `console.exception` and `console.trace` by replacing them with empty functions. This makes the use of the debugger harder.

### `domainLock`
Type: `string[]` Default: `[]`

##### :warning: This option does not work with `target: 'node'`

Locks the obfuscated source code so it only runs on specific domains and/or sub-domains. This makes really hard for someone to just copy and paste your source code and run it elsewhere.

##### Multiple domains and sub-domains
It's possible to lock your code to more than one domain or sub-domain. For instance, to lock it so the code only runs on **www.example.com** add `www.example.com`. To make it work on any sub-domain from example.com, use `.example.com`.

### `exclude`
Type: `string[]` Default: `[]`

A file names or globs which indicates files to exclude from obfuscation. 

### `identifierNamesGenerator`
Type: `string` Default: `hexadecimal`

Sets identifier names generator.

Available values:
* `dictionary`: identifier names from [`identifiersDictionary`](#identifiersDictionary) list
* `hexadecimal`: identifier names like `_0xabc123`
* `mangled`: short identifier names like `a`, `b`, `c`

### `identifiersDictionary`
Type: `string[]` Default: `[]`

Sets identifiers dictionary for [`identifierNamesGenerator`](#identifierNamesGenerator): `dictionary` option. Each identifier from the dictionary will be used in a few variants with a different casing of each character. Thus, the number of identifiers in the dictionary should depend on the identifiers amount at original source code.

### `identifiersPrefix`
Type: `string` Default: `''`

Sets prefix for all global identifiers.

Use this option when you want to obfuscate multiple files. This option helps to avoid conflicts between global identifiers of these files. Prefix should be different for every file.

### `inputFileName`
Type: `string` Default: `''`

Allows to set name of the input file with source code. This name will used internally for source map generation.

### `log`
Type: `boolean` Default: `false`

Enables logging of the information to the console.

### `renameGlobals`
Type: `boolean` Default: `false`

##### :warning: this option can break your code. Enable it only if you know what it does!

Enables obfuscation of global variable and function names **with declaration**.

### `renameProperties`
Type: `boolean` Default: `false`

##### :warning: this option **WILL** break your code in most cases. Enable it only if you know what it does!

Enables renaming of property names. All built-in DOM properties and properties in core JavaScript classes will be ignored.

To set format of renamed property names use [`identifierNamesGenerator`](#identifierNamesGenerator) option.

To control which properties will be renamed use [`reservedNames`](#reservedNames) option.

Example: 
```ts
// input
(function () {
    const foo = {
        prop1: 1,
        prop2: 2,
        calc: function () {
            return this.prop1 + this.prop2;
        }
    };
    
    console.log(foo.calc());
})();

// output
(function () {
    const _0x46529b = {
        '_0x10cec7': 0x1,
        '_0xc1c0ca': 0x2,
        '_0x4b961d': function () {
            return this['_0x10cec7'] + this['_0xc1c0ca'];
        }
    };
    console['log'](_0x46529b['_0x4b961d']());
}());
```

### `reservedNames`
Type: `string[]` Default: `[]`

Disables obfuscation and generation of identifiers, which being matched by passed RegExp patterns.

Example:
```ts
	{
		reservedNames: [
			'^someVariable',
			'functionParameter_\d'
		]
	}
```

### `reservedStrings`
Type: `string[]` Default: `[]`

Disables transformation of string literals, which being matched by passed RegExp patterns.

Example:
```ts
	{
		reservedStrings: [
			'react-native',
			'\.\/src\/test',
			'some-string_\d'
		]
	}
```

### `rotateStringArray`
Type: `boolean` Default: `true`

##### :warning: [`stringArray`](#stringarray) must be enabled

Shift the `stringArray` array by a fixed and random (generated at the code obfuscation) places. This makes it harder to match the order of the removed strings to their original place.

This option is recommended if your original source code isn't small, as the helper function can attract attention.

### `seed`
Type: `string|number` Default: `0`

This option sets seed for random generator. This is useful for creating repeatable results.

If seed is `0` - random generator will work without seed.

### `selfDefending`
Type: `boolean` Default: `false`

##### :warning: Don't change obfuscated code in any way after obfuscation with this option, because any change like uglifying of code can trigger self defending and code wont work anymore!
##### :warning: This option forcibly sets `compact` value to `true`

This option makes the output code resilient against formatting and variable renaming. If one tries to use a JavaScript beautifier on the obfuscated code, the code won't work anymore, making it harder to understand and modify it.

### `shuffleStringArray`
Type: `boolean` Default: `true`

##### :warning: [`stringArray`](#stringarray) must be enabled

Randomly shuffles the `stringArray` array items.

### `sourceMap`
Type: `boolean` Default: `false`

Enables source map generation for obfuscated code.

Source maps can be useful to help you debug your obfuscated JavaScript source code. If you want or need to debug in production, you can upload the separate source map file to a secret location and then point your browser there. 


### `sourceMapMode`
Type: `string` Default: `separate`

Specifies source map generation mode:
* `inline` - emit a single file with source maps instead of having a separate file;
* `separate` - generates corresponding '.map' file with source map. In case you run obfuscator through CLI - adds link to source map file to the end of file with obfuscated code `//# sourceMappingUrl=file.js.map`.

### `splitStrings`
Type: `boolean` Default: `false`

Splits literal strings into chunks with length of [`splitStringsChunkLength`](#splitStringsChunkLength) option value.

Example:
```ts
// input
(function(){
    var test = 'abcdefg';
})();

// output
(function(){
    var _0x5a21 = 'ab' + 'cd' + 'ef' + 'g';
})();
```

### `splitStringsChunkLength`
Type: `number` Default: `10`

Sets chunk length of [`splitStrings`](#splitStrings) option.

### `stringArray`
Type: `boolean` Default: `true`

Removes string literals and place them in a special array. For instance, the string `"Hello World"` in `var m = "Hello World";` will be replaced with something like `var m = _0x12c456[0x1];`
    
### `stringArrayEncoding`
Type: `boolean|string` Default: `false`

##### :warning: `stringArray` option must be enabled

This option can slow down your script.

Encode all string literals of the [`stringArray`](#stringarray) using `base64` or `rc4` and inserts a special code that used to decode it back at runtime.

Available values:
* `true` (`boolean`): encode `stringArray` values using `base64`
* `false` (`boolean`): don't encode `stringArray` values
* `'base64'` (`string`): encode `stringArray` values using `base64`
* `'rc4'` (`string`): encode `stringArray` values using `rc4`. **About 30-50% slower than `base64`, but more harder to get initial values.** It is recommended to disable [`unicodeEscapeSequence`](#unicodeescapesequence) option with `rc4` encoding to prevent very large size of obfuscated code.
    
### `stringArrayThreshold`
Type: `number` Default: `0.8` Min: `0` Max: `1`

##### :warning: [`stringArray`](#stringarray) option must be enabled

You can use this setting to adjust the probability (from 0 to 1) that a string literal will be inserted into the `stringArray`.

This setting is especially useful for large code size because it repeatedly calls to the `string array` and can slow down your code.

`stringArrayThreshold: 0` equals to `stringArray: false`.

### `target`
Type: `string` Default: `browser`

Allows to set target environment for obfuscated code.

Available values: 
* `browser`;
* `browser-no-eval`;
* `node`.

Currently output code for `browser` and `node` targets is identical, but some browser-specific options are not allowed to use with `node` target.
Output code for `browser-no-eval` target is not using `eval`.

### `transformObjectKeys`
Type: `boolean` Default: `false`

Enables transformation of object keys.

Example:
```ts
// input
(function(){
    var object = {
        foo: 'test1',
        bar: {
            baz: 'test2'
        }
    };
})();

// output
var _0x2fae = [
    'baz',
    'test2',
    'foo',
    'test1',
    'bar'
];
var _0x377c = function (_0x1fbd3f, _0x59c72f) {
    _0x1fbd3f = _0x1fbd3f - 0x0;
    var _0x14fada = _0x2fae[_0x1fbd3f];
    return _0x14fada;
};
(function () {
    var _0x8a12db = {};
    _0x8a12db[_0x377c('0x0')] = _0x377c('0x1');
    var _0xc75419 = {};
    _0xc75419[_0x377c('0x2')] = _0x377c('0x3');
    _0xc75419[_0x377c('0x4')] = _0x8a12db;
    var _0x191393 = _0xc75419;
}());
```

### `unicodeEscapeSequence`
Type: `boolean` Default: `false`

Allows to enable/disable string conversion to unicode escape sequence.

Unicode escape sequence increases code size greatly and strings easily can be reverted to their original view. Recommended to enable this option only for small source code. 

## Preset Options
### High obfuscation, low performance

Performance will 50-100% slower than without obfuscation

```javascript
{
    compact: true,
    controlFlowFlattening: true,
    controlFlowFlatteningThreshold: 1,
    deadCodeInjection: true,
    deadCodeInjectionThreshold: 1,
    debugProtection: true,
    debugProtectionInterval: true,
    disableConsoleOutput: true,
    identifierNamesGenerator: 'hexadecimal',
    log: false,
    renameGlobals: false,
    rotateStringArray: true,
    selfDefending: true,
    shuffleStringArray: true,
    splitStrings: true,
    splitStringsChunkLength: 5,
    stringArray: true,
    stringArrayEncoding: 'rc4',
    stringArrayThreshold: 1,
    transformObjectKeys: true,
    unicodeEscapeSequence: false
}
```

### Medium obfuscation, optimal performance

Performance will 30-35% slower than without obfuscation

```javascript
{
    compact: true,
    controlFlowFlattening: true,
    controlFlowFlatteningThreshold: 0.75,
    deadCodeInjection: true,
    deadCodeInjectionThreshold: 0.4,
    debugProtection: false,
    debugProtectionInterval: false,
    disableConsoleOutput: true,
    identifierNamesGenerator: 'hexadecimal',
    log: false,
    renameGlobals: false,
    rotateStringArray: true,
    selfDefending: true,
    shuffleStringArray: true,
    splitStrings: true,
    splitStringsChunkLength: 10,
    stringArray: true,
    stringArrayEncoding: 'base64',
    stringArrayThreshold: 0.75,
    transformObjectKeys: true,
    unicodeEscapeSequence: false
}
```

### Low obfuscation, High performance

Performance will slightly slower than without obfuscation

```javascript
{
    compact: true,
    controlFlowFlattening: false,
    deadCodeInjection: false,
    debugProtection: false,
    debugProtectionInterval: false,
    disableConsoleOutput: true,
    identifierNamesGenerator: 'hexadecimal',
    log: false,
    renameGlobals: false,
    rotateStringArray: true,
    selfDefending: true,
    shuffleStringArray: true,
    splitStrings: false,
    stringArray: true,
    stringArrayEncoding: false,
    stringArrayThreshold: 0.75,
    unicodeEscapeSequence: false
}
```

## Frequently Asked Questions

### What javascript versions are supported?

`es3`, `es5`, `es2015`, `es2016` and `es2017`

### I want to use feature that described in `README.md` but it's not working!

The README on the master branch might not match that of the latest stable release.



### Error `maximum call stack size exceeded`
Likely this is `selfDefending` mechanism. Something is changing source code after obfuscation with `selfDefending` option.



### How to change kind of variables of inserted nodes (`var`, `let` or `const`)?

See: [`Kind of variables`](#kind-of-variables)

### Why I got `null` value instead of `BigInt` number?

`BigInt` obfuscation works correctly only in environments that support `BigInt` values. See [ESTree spec](https://github.com/estree/estree/blob/master/es2020.md#bigintliteral)

See: [`Kind of variables`](#kind-of-variables)

### I enabled `renameProperties` option, and my code broke! What to do?

Just disable this option.


