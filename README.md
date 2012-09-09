# PhantomJS Runners for Mocha

[Mocha](http://visionmedia.github.com/mocha/) is a feature-rich JavaScript test framework running on node and the browser. Along with the [Chai](http://chaijs.com) assertion library they make an impressive combo. [PhantomJS](http://phantomjs.org) is a headless WebKit with a JavaScript/CoffeeScript API. It has fast and native support for various web standards like DOM handling, CSS selectors, JSON, Canvas, and SVG.

The mocha-phantomjs project provides a `mocha-phantomjs.coffee` script file and extensions to drive PhantomJS while testing your HTML pages with Mocha. All form the console.


# Key Features

### Standard Out

Finally `process.stdout.write`, done right. Mocha is primarly written for node, hence it relies on writing to standard out without trailing newline characters. This behavior is critical for reporters like the dot reporter. We make up for PhantomJS's lack of stream support by both customizing `console.log` and creating a `process.stdout.write` function to the current PhantomJS process. This technique combined with a handful of fancy [ANSI cursor movement codes](http://web.mit.edu/gnu/doc/html/screen_10.html) allows PhantomJS to support Mocha's diverse reporter options.

### Exit Codes

Proper exit status codes from PhantomJS using Mocha's failures. The number of Mocha's test failures from Mocha will be the return code. So in standard unix fashion, a `0` code means success. This means you can use mocha-phantomjs on your CI server of choice.

### Mixed Mode Runs

Mixed mode HTML file support. Meaning you can use your existing Mocha HTML file reporters side by side with mocha-phantomjs. This gives you the option to run your tests both in a browser or with PhantomJS. Since mocha-phantomjs needs to control when the `run()` command is sent to the mocha object, we accomplish this by setting the `mochaPhantomJS` on the `window` object to `true`. Here is an example of a HTML structure that can be used both by opening the file in your browser or choice or using mocha-phantomjs.

```html
<html>
  <head>
    <meta charset="utf-8">
    <link rel="stylesheet" href="mocha.css" />
  </head>
  <body>
    <div id="mocha"></div>
    <script src="mocha.js"></script>
    <script src="chai.js"></script>
    <script>
      mocha.ui('bdd'); 
      mocha.reporter('html');
      expect = chai.expect;
    </script>
    <script src="test/mycode.js"></script>
    <script>
      if (!window.mochaPhantomJS) { mocha.run(); }
    </script>
  </body>
</html>
```


# Usage

The `mocha-phantomjs.coffee` script should have be accompanied by the `mocha-phantomjs` directory included in the Github's `lib` project directory. This directory contains core extensions the script needs to inject into the loaded URL. If the `mocha-phantomjs.coffee` file resides directly beside this directory, everything should work. Basic usage:

```
$ phantomjs mocha-phantomjs.coffee URL [REPORTER]
```

Please see the supported reporter options below.


# Supported Reporters

Supporting Mocha reporters in Phantom

<div style="text-align:center;">
  <img src="https://raw.github.com/metaskills/mocha-phantomjs/master/public/images/slow.gif" alt="Slow Tests Example">
</div>

The following is a list of supported runners. If you do not see one that you really want, fork the repo and try to write the `customizeProcessStdout()` and `customizeConsole()` functions to support that runner. If you need help, [create a github issue](https://github.com/metaskills/mocha-phantomjs/issues) and let me know.

### Spec Reporter (default)

The default reporter. You can force it using `spec` for the reporter argument.

<div style="text-align:center;">
  <img src="https://raw.github.com/metaskills/mocha-phantomjs/master/public/images/reporter_spec.gif" alt="Spec Reporter" width="616">
</div>

### Dot Matrix Reporter

Use `dot` for the reporter argument. This reporter works best if you specific the `COLUMNS` environment variable.

<div style="text-align:center;">
  <img src="https://raw.github.com/metaskills/mocha-phantomjs/master/public/images/reporter_dot.gif" alt="Dot Matrix Reporter" width="616">
</div>

### TAP Reporter

Use `tap` for the reporter argument.

<div style="text-align:center;">
  <img src="https://raw.github.com/metaskills/mocha-phantomjs/master/public/images/reporter_tap.gif" alt="TAP Reporter" width="616">
</div>

### Min Reporter

Use `min` for the reporter argument.

<div style="text-align:center;">
  <img src="https://raw.github.com/metaskills/mocha-phantomjs/master/public/images/reporter_min.gif" alt="Min Reporter" width="616">
</div>

### List Reporter

Use `list` for the reporter argument.

<div style="text-align:center;">
  <img src="https://raw.github.com/metaskills/mocha-phantomjs/master/public/images/reporter_list.gif" alt="List Reporter" width="616">
</div>

### Doc Reporter

Use `doc` for the reporter argument.

<div style="text-align:center;">
  <img src="https://raw.github.com/metaskills/mocha-phantomjs/master/public/images/reporter_doc.gif" alt="Doc Reporter" width="616">
</div>


# Testing

Simple! Just clone the repo, then run `npm install` and the various node development dependencies will install to the `node_modules` directory of the project. If you have not done so, it is typically a good idea to add `/node_modules/.bin` to your `$PATH` so these modules bins are used. Now run `cake test` to start off the test suite.

We also use Travis CI to run our tests too. Current build status is:

[![Build Status](https://secure.travis-ci.org/metaskills/mocha-phantomjs.png)](http://travis-ci.org/metaskills/mocha-phantomjs)


# TODO

* Real Package - https://github.com/jamescarr/jasmine-tool
* Make sure runner hooks into status code. Test.


# License

Released under the MIT license. Copyright (c) 2012 Ken Collins.
