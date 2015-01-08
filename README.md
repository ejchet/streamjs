Stream.js [![Travic CI](https://travis-ci.org/winterbe/streamjs.svg?branch=master)](https://travis-ci.org/winterbe/streamjs)
========================

> An Object Streaming Pipeline for JavaScript - inspired by the [Java 8 Streams API](http://winterbe.com/posts/2014/07/31/java8-stream-tutorial-examples/)

```js
Stream([5, 9, 2, 4, 8, 1])
   .filter(function (num) {
      return num % 2 === 1;
   })
   .sorted()
   .map(function (num) {
      return "odd" + num;
   })
   .toArray();
```

<p align="center">
   <i>Follow on <a href="https://twitter.com/benontherun">Twitter</a> for Updates</i>
</p>

# Getting started

Stream.js can be installed manually by downloading the [latest release](https://github.com/winterbe/streamjs/releases) from GitHub. The `dist` folder contains both the minified script and a source map file. Alternatively you can install Stream.js with [Bower](http://bower.io/):

```bash
$ bower install streamjs
```

# How Streams work

Stream.js defines a single function `Stream` to create new streams from different input sources like _arrays_, _maps_ or _number ranges_. Streams are monadic types with a bunch of useful operations. Those functions can be chained one after another to make complex computations upon the input elements. Operations are either *intermediate* or *terminal*. Intermediate operations are lazy return the stream itself to enable method chaining. Terminal operations return a single result (or nothing at all). Some terminal operations return a special monadic `Optional` type which is described later.

# Why Stream.js?

What's so different between Stream.js and other functional libraries like Underscore.js?

Stream.js is built around a lazily evaluated operation pipeline. Instead of consecutively performing each operation on the whole input collection, objects are passed vertically and one by one upon the chain. Interim results will _not_ be stored in internal collections (except for some stateful operations like `sorted`). Instead objects are directly piped into the resulting object as specified by the terminal operation. This results in **minimized memory consumption** and internal state.

Stream operations are lazily evaluated to avoid examining all of the input data when it's not necessary. Streams always perform the minimal amount of operations to gain results. E.g. in a `filter - map - findFirst` stream you don't have to filter and map the whole data. Instead `map` and `findFirst` will only executed once before returning the single result. This results in **increased performance** when operation upon large amounts of input elements.

```js
Stream([1, 2, 3, 4])
   .filter(function(num) {   // called twice
      return num % 2 === 0;
   })
   .map(function(even) {     // called once
      return "even" + even;
   })
   .findFirst();             // called once
```

# API Doc

## Constructors

The following constructor functions can be used to create different kind of streams.

##### Stream(collection)

Returns a new stream for the given collection. Collection can either be an array or an object hash (map).

##### Stream.range(startInclusive, endExclusive)

Returns a new stream for the given number range.

##### Stream.rangeClosed(startInclusive, endInclusive)

Returns a new stream for the given number range.

## Intermediate Operations

Intermediate operations all return a stream, thus enabling you to chain multiple operations one after another without using semicolons.

##### filter(predicate)

Filters the elements of the stream to match the given predicate function and returns the stream.

##### map(mappingFn)

Applies the given mapping function to each element of the stream and returns the stream.

##### flatMap(flatMappingFn)

Applies the given mapping function to each element of the stream and replaces the elements with the contents of returned values, then returns the stream.

##### sorted()

Sorts the elements of the stream according to the natural order and returns the stream.

##### sorted(comparator)

Sorts the elements of the stream according to the given comparator and returns the stream.

##### distinct()

Returns the stream consisting of the distinct elements of this stream.

##### limit(maxSize)

Truncates the elements of the stream to be no longer than `maxSize` and returns the stream.

##### skip(n)

Discards the first `n` elements and returns the stream.

##### peek(consumer)

Performs the consumer function for each element and returns the stream.

## Terminal Operations

Terminal operations return a result (or nothing), so each streaming pipeline consists of 0 to n intermediate operations followed by exactly one terminal operation.

##### toArray()

Returns an array containing the elements of the stream.

##### forEach(consumer)

Performs the consumer function for each element of the stream.

##### findFirst()

Returns an `Optional` wrapping the first element of the stream or `Optional.empty()` if the stream is empty.

##### min()

Returns an `Optional` wrapping the minimum element of the stream (according the natural order) or `Optional.empty()` if the stream is empty.

##### min(comparator)

Returns an `Optional` wrapping the minimum element of the stream (according the given comparator function) or `Optional.empty()` if the stream is empty.

##### max()

Returns an `Optional` wrapping the maximum element of the stream (according the natural order) or `Optional.empty()` if the stream is empty.

##### max(comparator)

Returns an `Optional` wrapping the maximum element of the stream (according the given comparator function) or `Optional.empty()` if the stream is empty.

##### sum()

Returns the sum of all elements in this stream.

##### average()

Returns an `Optional` wrapping the arithmetic mean of all elements of this stream or `Optional.empty()` if the stream is empty.

##### count()

Returns the number of elements of the stream.

##### allMatch(predicate)

Returns whether all elements of the stream match the given predicate function.

##### anyMatch(predicate)

Returns whether any element of the stream matches the given predicate function.

##### noneMatch(predicate)

Returns whether no element of the stream matches the given predicate function.

##### collect(collector)

Performs a generalized reduction operation denoted by the given collector and returns the reduced value. A collector consists of a supplier function, an accumulator function and an optional finisher function.

##### reduce(identity, accumulator)

Performs a reduction operation using the provided `identity` object as initial value and the accumulator function and returns the reduced value.

##### reduce(accumulator)

Performs a reduction operation using the first element of the stream as as initial value and the accumulator function and returns the reduced value wrapped as `Optional`.

##### groupBy(keyMapper)

Groups all elements of the stream by applying the given keyMapper function and returns an object map, assigning an array value for each key.

##### indexBy(keyMapper, mergeFunction)

Groups all elements of the stream by applying the given keyMapper function and returns an object map, assigning a single value for each key. Multiple values for the same key will be merged using the given merge function.

##### partitionBy(predicate)

##### partitionBy(size)

##### joining(options)
 
## Stream.Optional

##### Optional.of(value)
##### Optional.ofNullable(value)
##### Optional.empty()
##### get()
##### isPresent()
##### ifPresent(consumer)
##### orElse(value)
##### orElseGet(supplier)
##### orElseThrow(error)
##### filter(predicate)
##### map(mappingFn)
##### flatMap(mappingFn)

# Compatibility

Stream.js targets ECMAScript 5 javascript engines and is tested in all modern browsers. See `test` folder for a bunch of automated QUnit tests.

# Copyright and license

Created and copyright (c) 2014-2015 by Benjamin Winterberg. Stream.js may be freely distributed under the MIT license.
