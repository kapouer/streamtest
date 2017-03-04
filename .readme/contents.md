`streamtest` is a set of utils to test your stream based modules accross various
 stream implementations of NodeJS.

In fact, this [repository's tests](https://github.com/nfroidure/streamtest/blob/master/tests/index.mocha.js)
 is a perfect example of its own usage ;).

## Installation

First install `streamtest` in your project:
```sh
npm install --save-dev streamtest
```

## Getting started

Then, use it:

```js
var StreamTest = require('streamtest');

describe('My Stream Lib', function() {

  // Iterating through versions
  StreamTest.versions.forEach(function(version) {

    describe('for ' + version + ' streams', function() {

      // here goes your code
      it('should work', function(done) {

        StreamTest[version].fromChunks(['a ', 'chunk', 'and', 'another'])
          .pipe(myTestedStream)
          .pipe(StreamTest[version].toText(function(err, text) {
            if(err) {
              done(err);
            }
            assert.equal(text, 'the text as i should be');
            done();
          }));

      });

    });

  });

});
```

## API

### StreamTest.versions:Array
List of supported versions (currently v1 and v2).

### StreamTest[version]:Object
Object available for each version containing the following methods.

### StreamTest[version].fromChunks(chunks:Array, timeout:Number)

Create a readable stream streaming `chunks` each `timeout` milliseconds and then
 end. Usefull for testing buffer based streams.

### StreamTest[version].fromObjects(objects:Array, timeout:Number)

Create a readable stream streaming `objects` each `timeout` milliseconds and
 then end. Usefull for testing objectMode based streams.

### StreamTest[version].fromErroredChunks(err:Error, chunks:Array, timeout:Number)

Create a readable stream streaming `chunks` each `timeout` milliseconds, emit
 the `err` error and then end. Usefull for testing buffer based streams.

### StreamTest[version].fromErroredObjects(err:Error, objects:Array, timeout:Number)

Create a readable stream streaming `objects` each `timeout` milliseconds, emit
 the `err` error and then end. Usefull for testing objectMode based streams.

### StreamTest[version].toChunks(cb:Function)

Create a writable stream collecting written `chunks` and calling the `cb`
 function when it finishes.

The `cb` function take in an error and an Array of chunks.

### StreamTest[version].toObjects(cb:Function)

Create a writable stream collecting written `chunks` and calling the `cb`
 function when it finishes.

The `cb` function take in an error and an Array of objects.

### StreamTest[version].toText(cb:Function)

Create a writable stream collecting written `chunks` and calling the `cb`
 function when it finishes with the whole content converted to text.

The `cb` function take in an error and a string.

## Contribute

Feel free to submit us your improvements. To do so, you must accept to publish
 your code under the MIT license.

To start contributing, first run the following to setup the development
 environment:
```sh
git clone git@github.com:nfroidure/streamtest.git
cd streamtest
npm install
```

Then, run the tests:
```sh
npm test
```

## Stats
[![NPM](https://nodei.co/npm/streamtest.png?downloads=true&stars=true)](https://nodei.co/npm/streamtest/)
[![NPM](https://nodei.co/npm-dl/streamtest.png)](https://nodei.co/npm/streamtest/)