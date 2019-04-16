---
npm install bs58@4.0.1 bytebuffer@5.0.1 core-js@2.5.0 node-fetch@2.1.2 secp256k1@3.3.1 verror@1.10.0 whatwg-fetch@2.0.3   
npm install bs58@4.0.1 bytebuffer@5.0.1 core-js@2.5.0 node-fetch@2.1.2 secp256k1@3.3.1 verror@1.10.0 whatwg-fetch@2.0.3 -g   

---
---
npm install @types/bytebuffer@5.0.33 @types/mocha@5.2.2 @types/node@10.3.2 @types/verror@1.10.3   
npm install @types/bytebuffer@5.0.33 @types/mocha@5.2.2 @types/node@10.3.2 @types/verror@1.10.3 -g   

---
---
npm install babel-core@6.26.0 babel-preset-env@1.6.1 babelify@8.0.0 browserify@16.2.2 coveralls@3.0.0 derequire@2.0.6 dts-generator@2.1.0 encoding@0.1.12 karma@2.0.2 karma-browserify@5.1.1 karma-mocha@1.3.0 karma-mocha-reporter@2.2.5 karma-sauce-launcher@1.2.0 lorem-ipsum@1.0.4 mocha@5.0.0 nyc@12.0.2 regenerator-runtime@0.11.0 ts-node@6.1.1 tsify@4.0.0 tslint@5.10.0 typedoc@0.11.1 typescript@2.9.1 uglify-js@3.4.0 watchify@3.11.0   
npm install babel-core@6.26.0 babel-preset-env@1.6.1 babelify@8.0.0 browserify@16.2.2 coveralls@3.0.0 derequire@2.0.6 dts-generator@2.1.0 encoding@0.1.12 karma@2.0.2 karma-browserify@5.1.1 karma-mocha@1.3.0 karma-mocha-reporter@2.2.5 karma-sauce-launcher@1.2.0 lorem-ipsum@1.0.4 mocha@5.0.0 nyc@12.0.2 regenerator-runtime@0.11.0 ts-node@6.1.1 tsify@4.0.0 tslint@5.10.0 typedoc@0.11.1 typescript@2.9.1 uglify-js@3.4.0 watchify@3.11.0 -g

---




# [dsteem](https://github.com/jnordberg/dsteem) [![Build Status](https://img.shields.io/circleci/project/github/jnordberg/dsteem.svg?style=flat-square)](https://circleci.com/gh/jnordberg/workflows/dsteem) [![Coverage Status](https://img.shields.io/coveralls/jnordberg/dsteem.svg?style=flat-square)](https://coveralls.io/github/jnordberg/dsteem?branch=master) [![Package Version](https://img.shields.io/npm/v/dsteem.svg?style=flat-square)](https://www.npmjs.com/package/dsteem)

Robust [steem blockchain](https://steem.io) client library that runs in both node.js and the browser.

* [Demo](https://comments.steem.vc) ([source](https://github.com/jnordberg/dsteem/tree/master/examples/comment-feed))
* [Code playground](https://playground.steem.vc)
* [Documentation](https://jnordberg.github.io/dsteem/)
* [Bug tracker](https://github.com/jnordberg/dsteem/issues)

---

**note** As of version 0.7.0 WebSocket support has been removed. The only transport provided now is HTTP(2). For most users the only change required is to swap `wss://` to `https://` in the address. If you run your own full node make sure to set the proper [CORS headers](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) if you plan to access it from a browser.

---


Browser compatibility
---------------------

[![Build Status](https://saucelabs.com/browser-matrix/jnordberg-dsteem.svg)](https://saucelabs.com/open_sauce/user/jnordberg-dsteem)


Installation
------------

### Via npm

For node.js or the browser with [browserify](https://github.com/substack/node-browserify) or [webpack](https://github.com/webpack/webpack).

```
npm install dsteem
```

### From cdn or self-hosted script

Grab `dist/dsteem.js` from a [release](https://github.com/jnordberg/dsteem/releases) and include in your html:

```html
<script src="dsteem.js"></script>
```

Or from the [unpkg](https://unpkg.com) cdn:

```html
<script src="https://unpkg.com/dsteem@^0.8.0/dist/dsteem.js"></script>
```

Make sure to set the version you want when including from the cdn, you can also use `dsteem@latest` but that is not always desirable. See [unpkg.com](https://unpkg.com) for more information.


Usage
-----

### In the browser

```html
<script src="https://unpkg.com/dsteem@latest/dist/dsteem.js"></script>
<script>
    var client = new dsteem.Client('https://api.steemit.com')
    client.database.getDiscussions('trending', {tag: 'writing', limit: 1}).then(function(discussions){
        document.body.innerHTML += '<h1>' + discussions[0].title + '</h1>'
        document.body.innerHTML += '<h2>by ' + discussions[0].author + '</h2>'
        document.body.innerHTML += '<pre style="white-space: pre-wrap">' + discussions[0].body + '</pre>'
    })
</script>
```

See the [demo source](https://github.com/jnordberg/dsteem/tree/master/examples/comment-feed) for an example on how to setup a livereloading TypeScript pipeline with [wintersmith](https://github.com/jnordberg/wintersmith) and [browserify](https://github.com/substack/node-browserify).

### In node.js

With TypeScript:

```typescript
import {Client} from 'dsteem'

const client = new Client('https://api.steemit.com')

for await (const block of client.blockchain.getBlocks()) {
    console.log(`New block, id: ${ block.block_id }`)
}
```

With JavaScript:

```javascript
var dsteem = require('dsteem')

var client = new dsteem.Client('https://api.steemit.com')
var key = dsteem.PrivateKey.fromLogin('username', 'password', 'posting')

client.broadcast.vote({
    voter: 'username',
    author: 'almost-digital',
    permlink: 'dsteem-is-the-best',
    weight: 10000
}, key).then(function(result){
   console.log('Included in block: ' + result.block_num)
}, function(error) {
   console.error(error)
})
```

With ES2016 (node.js 7+):

```javascript
const {Client} = require('dsteem')

const client = new Client('https://api.steemit.com')

async function main() {
    const props = await client.database.getChainProperties()
    console.log(`Maximum blocksize consensus: ${ props.maximum_block_size } bytes`)
    client.disconnect()
}

main().catch(console.error)
```

With node.js streams:

```javascript
var dsteem = require('dsteem')
var es = require('event-stream') // npm install event-stream
var util = require('util')

var client = new dsteem.Client('https://api.steemit.com')

var stream = client.blockchain.getBlockStream()

stream.pipe(es.map(function(block, callback) {
    callback(null, util.inspect(block, {colors: true, depth: null}) + '\n')
})).pipe(process.stdout)
```


Bundling
--------

The easiest way to bundle dsteem (with browserify, webpack etc.) is to just `npm install dsteem` and `require('dsteem')` which will give you well-tested (see browser compatibility matrix above) pre-bundled code guaranteed to JustWork™. However, that is not always desirable since it will not allow your bundler to de-duplicate any shared dependencies dsteem and your app might have.

To allow for deduplication you can `require('dsteem/lib/index-browser')`, or if you plan to provide your own polyfills: `require('dsteem/lib/index')`. See `src/index-browser.ts` for a list of polyfills expected.

---

*Share and Enjoy!*
