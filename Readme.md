
[![NPM version][npm-img]][npm-url]
[![License][license-img]][license-url]

### koa-response-cache

a middleware for koa to cache response with redis.

extend from `koa-redis-cache` 

[https://github.com/coderhaoxin/koa-redis-cache](https://github.com/coderhaoxin/koa-redis-cache)


### Installation
```bash
$ npm install koa-response-cache
```

### Example

```js
const cache = require('koa-response-cache')
const koa = require('koa')
const app = koa()

let options = {
  expire: 60,
  routes: ['/index'],
  condition: function (ctx) {
      const userAgent = ctx.headers['user-agent'] || '';
      const match = userAgent.match(/(iphone|ipod|ipad|android|phone|pad|pod|mobile)/ig);
      return !match;
  },
}

app.use(cache(options))
```

### options

* prefix
  - type: `String` or `Function`
  - redis key prefix, default is `koa-response-cache:`
  - If a function is supplied, its signature should be `function(ctx) {}` and it should return a string to use as the redis key prefix
* expire
  - type: `Number`
  - redis expire time (second), default is `30 * 60` (30 min)
* passParam
  - type: `String`
  - if the passParam is existed in query string, not get from cache
* maxLength
  - type: `Number`
  - max length of the body to cache
* routes
  - type: `Array`
  - the routes to cache, default is `['(.*)']`
  - It could be `['/api/(.*)', '/view/:id']`, see [path-to-regexp](https://github.com/pillarjs/path-to-regexp)
* exclude
  - type: `Array`
  - the routes to exclude, default is `[]`
  - It could be `['/api/(.*)', '/view/:id']`, see [path-to-regexp](https://github.com/pillarjs/path-to-regexp)
* onerror
  - type: `Function`
  - callback function for error, default is `function() {}`
* condition
  - type: `Function`
  - should be `function(ctx) {}` and it should return `true` or `false` to match redis cache condition
* redis
  - type: `Object`
  - redis options
* redis.port
  - type: `Number`
* redis.host
  - type: `String`
* redis.options
  - type: `Object`
  - see [node_redis](https://github.com/mranney/node_redis)

### set different expire for each route

```js
const cache = require('koa-response-cache')
const koa = require('koa')
const app = koa()

let options = {
  routes: [{
    path: '/index',
    expire: 60
  }, {
    path: '/user',
    expire: 5
  }]
}

app.use(cache(options))
```

### notes

* `koa-response-cache` will set a custom http header `X-Koa-Response-Cache: true` when the response is from cache

### License
MIT

[npm-img]: https://img.shields.io/npm/v/koa-response-cache.svg?style=flat-square
[npm-url]: https://npmjs.org/package/koa-response-cache
[license-img]: http://img.shields.io/badge/license-MIT-green.svg?style=flat-square
[license-url]: http://opensource.org/licenses/MIT