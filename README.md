# Promise Events

[![Build Status](https://travis-ci.org/yanickrochon/promise-events.svg?branch=master)](https://travis-ci.org/yanickrochon/promise-events)
[![Coverage Status](https://coveralls.io/repos/yanickrochon/promise-events/badge.svg)](https://coveralls.io/r/yanickrochon/promise-events)

[![NPM](https://nodei.co/npm/promise-events.png?compact=true)](https://nodei.co/npm/promise-events/)

An asynchronous event listener for Promise/A+ implementations. This module implements a compatible `EventEmitter` interface, where all functions return a promise for easy workflow.

The emitter can work either synchronously or asynchrnously. However, all events are fired asynchronously.


### Usage


```
const EventEmitter = require('promise-events');

var events = new EventEmitter();

// synchrnous
events.on('syncEvent', function (hello) {
  console.log(hello);
});

events.emit('syncEvent', 'hello!');


// asynchronous
Promise.all([
  events.on('asyncEvent', function (hello) {
    console.log('Handler 1', hello);
    return 'Bye!';
  }),
  events.on('asyncEvent', function (hello) {
    console.log('Handler 2', hello);
  })
]).then(function () {
  console.log("Event added and any newListener listeners fired!");
}).then(function () {

  events.emit('asyncEvent', 'Hello async!').then(function (results) {
    console.log(results);
    // results = [ 'Bye!' ]
  });

});
```

All listeners are executed using [`Promise.all`](https://people.mozilla.org/~jorendorff/es6-draft.html#sec-promise.all), and empty results (listeners returning no result or `undefined`) are filtered out. The order of the items in `results` is undefined. Therefore, the number of listeners and the order they are added to the emitter do not garantee the order of values returned when emitting an event; do not rely on `results` to determine a listener's return value.


## Contribution

All contributions welcome! Every PR **must** be accompanied by their associated
unit tests!


## License

MIT