---
title: bluebird.js
category: JavaScript libraries
layout: default-ad
---

Also see the [promise cheatsheet](promise.html) and [Bluebird.js API](https://github.com/petkaantonov/bluebird/blob/master/API.md) (github.com).
{:.center.brief-intro}

```js
promise
  .then(okFn, errFn)
  .spread(okFn, errFn) //*
  .catch(errFn)
  .catch(TypeError, errFn) //*
  .finally(fn) //*
  .map(function (e) { ... })
  .each(function (e) { ... })
```

----

### Multiple return values
Use [Promise.spread](http://bluebirdjs.com/docs/api/promise.spread.html).

```js
.then(function () {
  return [ 'abc', 'def' ];
})
.spread(function (abc, def) {
  ...
});
```

### Multiple promises
Use [Promise.join](http://bluebirdjs.com/docs/api/promise.join.html).

```js
Promise.join(
  getPictures(),
  getMessages(),
  getTweets(),
  function (pics, msgs, tweets) {
    return ...;
  }
)
```

### Multiple promises (array)

- [Promise.all](http://bluebirdjs.com/docs/api/promise.all.html)([p]) - expect all to pass
- [Promise.some](http://bluebirdjs.com/docs/api/promise.some.html)([p], count) - expect `count` to pass
- [Promise.any](http://bluebirdjs.com/docs/api/promise.any.html)([p]) - same as `some([p], 1)`
- [Promise.race](http://bluebirdjs.com/docs/api/promise.race.html)([p], count) - use `.any` instead
- [Promise.map](http://bluebirdjs.com/docs/api/promise.map.html)([p], fn, options) - supports concurrency

```js
Promise.all([ promise1, promise2 ])
  .then(results => {
    results[0]
    results[1]
  })

// succeeds if one succeeds first
Promise.any(promises)
  .then(results => {
  })
```

Use [Promise.map](http://bluebirdjs.com/docs/api/promise.map.html) to "promisify" a list of values.

```js
Promise.map(urls, url => fetch(url))
  .then(...)
```


### Object
Usually it's better to use `.join`, but whatever.

```js
Promise.props({
  photos: get('photos'),
  posts: get('posts')
})
.then(res => {
  res.photos
  res.posts
})
```

### Chain of promises
Use [Promise.try](http://bluebirdjs.com/docs/api/promise.try.html).

```js
function getPhotos() {
  return Promise.try(() => {
    if (err) throw new Error("boo")
    return result
  })
}

getPhotos().then(...)
```

### Using Node-style functions
See [Promisification](http://bluebirdjs.com/docs/api/promisification.html).

```js
var readFile = Promise.promisify(fs.readFile)
var fs = Promise.promisifyAll(require('fs'))
```

### Promise-returning methods
See [Promise.method](http://bluebirdjs.com/docs/api/promise.method.html).

```js
User.login = Promise.method((email, password) => {
  if (!valid)
    throw new Error("Email not valid")

  return /* promise */
});
```

### Generators
See [Promise.coroutine](http://bluebirdjs.com/docs/api/promise.coroutine.html).

```
User.login = Promise.coroutine(function* (email, password) {
  let user = yield User.find({email: email}).fetch()
  return user
})
```

## Reference

<http://bluebirdjs.com/docs/api-reference.html>
