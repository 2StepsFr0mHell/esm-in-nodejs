# ES Modules in Node Today!

The ESM (EcmaScript Modules) specification introduced with the ES6 (or ES2015) norm describes how to import and export modules in JavaScript.
From now, it is possible to use ESM almost natively… without the help of Babel!

## From CommonJS to ES Modules

With the [native ESM support in browser](https://jakearchibald.com/2017/es-modules-in-browsers/), it now seems essential to have it in `Node.js`. From its beginning, `Node.js` uses a module system called CommonJS. Here is a little syntax comparison:

### CJS:

```javascript
const a = require("./a")
module.exports = { a, b: 2 }
```

### ESM:

```javascript
import a from "./a"
export default { a, b: 2 }
```

They seem very similar, but migrating from one to the other is not so easy. In fact, these 2 syntaxes are incompatibles. For more details, you have to read the [excellent article](https://ponyfoo.com/articles/es6-modules-in-depth#the-es6-module-system) from Nicolás Bevacqua talking about that.

The Node.js contributors have decided to use a new file extension for the ESM syntax: the `.mjs` extension. Just adding a `m` for "modules" 😏. Easy, right?

Modules should be natively available in the Node.js version 10, in about April 2018. But waiting for this date, what can we do?

If you expose a library, you have many choices:

  - Ignore the existence of native modules and use CommonJS 🤠 (emoji failed?)
  - Use native modules with Babel transpiler 🤔
  - Expose both of these systems 😵

Node.js provides a full support of ES7 and ES8. Transpiling the code **only** to export and import modules through ESM syntax is such a shame, don't you think?


## @std/esm

[`@std/es`](https://www.smooth-code.com/articles/es6-modules-natif-nodejs) to the rescue! This package, written by [John Dalton](https://github.com/jdalton) (the `loadash` creator), allows to use ESM in Node.js without transpilation. It works without any trouble with an in-place ecosystem like Babel and Webpack.

## Use @std/esm in a project

You have first to install the library (for instance `yarn add @std/esm` or `npm install @std/esm`). Then you have to call the `require('@std/esm')` before importing ESM modules (with the `.mjs` extension if you followed the explanations 😜).

In the following example, there is a `multiply` module which should **export** the `multiply` function and a `main` file which should **import** this last module. Up to you to write the proper code 🤔 :

@[Use ES Module syntax]({ "stubs": ["multiply.mjs", "main.mjs"], "command": "node main.js", "language": "javascript" })

For your information, you have 2 ways to make this code running:
  - Use the `-r` option of node to load a module before launching the application: `$ node -r @std/esm main.mjs`
  - Create the `main.js` to serve as a "hatch" to launch the "right" `main.mjs`:
  ```javascript
    // main.js
    require = require('@std/esm')(module)
    require('./main.mjs')
  ```

Great, isn't it?! You can now remove Babel from your node projects and use all the ES6 functionalities (better late than never 🙄).

## More information about `@std/esm`
The library fully supports:

  - [import](https://ponyfoo.com/articles/es6-modules-in-depth#import) / [export](https://ponyfoo.com/articles/es6-modules-in-depth#export)
  - [`Dynamic import()`](https://github.com/tc39/proposal-dynamic-import)
  - [Live bindings](https://ponyfoo.com/articles/es6-modules-in-depth#bindings-not-values)
  - [`.mjs` file load](https://github.com/nodejs/node-eps/blob/master/002-es-modules.md#32-determining-if-source-is-an-es-module)

## Many thanks

I would like to thank [Greg Bergé](https://www.smooth-code.com/formateurs/greg-berge) from [smooth code](https://www.smooth-code.com/) who wrote [this article](https://www.smooth-code.com/articles/es6-modules-natif-nodejs).
