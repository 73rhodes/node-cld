# cldpre - a fork of node-cld
[![*NIX Build Status](https://secure.travis-ci.org/dachev/node-cld.png)](https://travis-ci.org/dachev/node-cld)
[![Windows Build Status](https://ci.appveyor.com/api/projects/status/github/dachev/node-cld?svg=true&retina=true)](https://ci.appveyor.com/project/dachev/node-cld)
[![Dependencies](https://david-dm.org/dachev/node-cld.png)](https://david-dm.org/dachev/node-cld)
[![NPM version](https://badge.fury.io/js/cld.svg)](http://badge.fury.io/js/cld)

A fork of dachev/node-cld with pre-built binaries for OSX and Linux. 

Language detection for Javascript. Based on the CLD2 (Compact Language Detector) library from Google.

Highly optimized for space and speed. Runs about 10x faster than other libraries. Detects over 160 languages. Full test coverage. Runs on Linux, OS X, and Windows.

## Installation

```bash
$ npm install cld
```

Linux users, make sure you have g++ >= 4.8. If this is not an option, you should be able to install node-cld 2.4.4 even with an older g++ build.

## Examples
### Simple
```js
const cldpre = require('cldpre');

// As a promise
cldpre.detect('This is a language recognition example').then((result) => {
  console.log(result);
});

// In an async function
async function testCld() {
  const result = await cldpre.detect('This is a language recognition example');
  console.log(result);
}
```

### Advanced
```js
const cldpre = require('cldpre');
const text     = 'Това е пример за разпознаване на Български език';
const options  = {
  isHTML       : false,
  languageHint : 'BULGARIAN',
  encodingHint : 'ISO_8859_5',
  tldHint      : 'bg',
  httpHint     : 'bg'
};

// As a promise
cldpre.detect(text, options).then((result) => {
  console.log(result);
});

// In an async function
async function testCld() {
  const result = await cldpre.detect(text, options);
  console.log(result);
}
```

### Legacy
Detect can be called leveraging the node callback pattern. If options are provided, the third parameter should be the callback.
```javascript
const cldpre = require('cldpre');

cldpre.detect('This is a language recognition example', (err, result) => {
  console.log(result);
});
```

## Options

#### isHTML

Set to true if the string contains HTML tags

#### languageHint

Pass a LANGUAGES key or value as a hint

#### encodingHint

Pass an ENCODINGS value as a hint

#### tldHint

Pass top level domain as a hint

#### httpHint

Pass an HTTP "Content-Encoding" value as a hint

## Warning
Once the module has been installed, the underlying C sources will remain in the ```deps/cldpre``` folder and continue to occupy considerable space. This is because they will be required if you ever need to run `npm rebuild`. If you are under severe constraints you can delete this folder and reclam >100M

## Prebuilds
This fork allows pre-built binaries to be bundled with the NPM package using `prebuildify` and `prebuildify-cross`.

After checking out this repository, compiling the prebuilt binaries on Mac OS X (darwin) would be done as follows:

```
prebuildify --napi --strip 
prebuildify-cross -i centos7-devtoolset7 --napi --strip
```

This will generate `prebuilds/linux-x86` and `prebuilds/darwin-x86` binaries. When publishing, the `cldpre` module contains the `prebuilds/` contents which are loaded by `node-pre-gyp` as required by the current platform. Falls back to rebuilding if prebuild for the current platform is not found.

## Copyright
Copyright 2011-2015, Blagovest Dachev.

## License
Apache 2
