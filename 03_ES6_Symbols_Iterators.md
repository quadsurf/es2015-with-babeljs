# New Features in ES6 Part 2

## Objectives

* Describe when you would use a symbol over a string
* Replace an indexed for loop with a for-of loop
* Iterate over a custom object

## Symbols

Symbols are the 7th _type_ to be added to Javascript, and the first new type to be added since it was first standardized. Symbols in Javascript operate  similarly to symbols in Ruby, but have some distinct differences and are used in different ways. Let's look at a few examples, then talk through how those work:

```javascript
let nameSymbol = Symbol('name');
let uniqueSymbol = Symbol('name');
let newNameSymbol = Symbol.for('name');
let sameNameSymbol = Symbol.for('name');
```

This will create 3 unique symbol objects. Using `Symbol.for` will register that symbol into the global symbol registry, or fetch the symbol with the same description from the global registry. This operates most similarly to Ruby, and when you want to ensure that you can recreate the same symbol, you will always want to use `Symbol.for`. The alternative syntax, just `Symbol(description)` will only use the description when converting that symbol to a string. Even if you pass in the same description, it will create a brand new symbol every time. It's still worthwhile to define descriptions on your symbols, since it will make debugging easier.

Symbols are *not* intended to be a drop in replacement for strings when creating objects. Properties associated with a symbol will be omitted from most of the normal ways of iterating over properties on an object.

```javascript
let instructor = {};
let name = Symbol.for('name');

instructor[name] = 'Chris';

Object.keys(instructor)
// []

console.log(instructor[Symbol.for('name')]);
// Chris

console.log(JSON.stringify(instructor));
// {}
```

So, if they aren't displayed when working with the object, what's the point? The main purpose of symbols in es2015 is to guarantee that you won't _collide_ with any existing properties on the object. You can create a key that is guaranteed to be unique to store extra data (or functions) that need to be added to an object. This helps to alleviate some of the  [https://github.com/facebook/react/blob/401e6f10587b09d4e725763984957cf309dfdc30/vendor/react-dom.js#L30](extreme measures) some libraries have taken to avoid conflicts.

There are a few [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol#Well-known_symbols](predefined symbols) that are included in es2015 that allow you to define functionality in custom objects. We'll show an example of one of these in the next section.

#### Exercise

Take a look at the [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol](symbol documentation) and determine how we can iterate over the symbol keys that we are using for our `instructor` object above.

## Iterators

The simplest way to iterate over an array in javascript looks something like this:

```javascript
let arr = [1, 2, 3];

for (let i = 0; i < arr.length; i++) {
  console.log(arr[i]);
}
```

This is OK, but we can do better. At the very least, we can switch this over to use the ES5 built in `forEach` method instead:

```javascript
arr.forEach(val => {
  console.log(val);
});
```

This is a bit better, but it introduces some new issues like the fact that we can't break out of this loop early. We have no choice but to iterate over every element in the array. Another potential solution is that we could use `for-in`, such as:

```javascript
for (let i in arr) {
  console.log(arr[i]);
}
```

This is better, but still has potentially weird behavior. It will cast the index as a string (since `for-in` is designed to work with objects, not arrays) which can potentially create some weird bugs if you intend to work with the index value as an integer directly. It will also iterate over the items in an arbitrary order, there's no guarantee that your indexes will be returned in the order you'd expect them to.

Instead, in es2015, we can use `for-of`:

```javascript
for (let val in arr) {
  console.log(val);
}
```

Now we are able to avoid all of these potential problems. Given an array, items will be returned in proper order, and we're now just working with the data directly instead of having indirect access to that data.

### What else can we do with `for-of`?

This statement works with lots of data types, not just arrays. Since strings can be treated like arrays, strings work just fine with `for-of` as well:

```javascript
for (let char of "hello") {
  console.log(char);
}
```

In addition, it works with the new `Map` and `Set` objects that we will describe later.

It does *not*, however, work with objects. Objects are intended to be iterated over with `for-in`.

### Iterating over our own types of data

What if we are building our own types of collections instead of just using built ins? The new iterator functionality in es2015 allows us to enable this `for-of` functionality for our own data types. Any object that provides a `[Symbol.iterator]()` method is _iterable_, and can be utilized with `for-of`.

Let's look at a very basic iterator:

```javascript
let zeroesForeverIterator = {
  [Symbol.iterator]() {
    return this;
  },
  next() {
    return {done: false, value: 0};
  }
};

for (let val of zeroesForeverIterator) {
  console.log(val);
}
```

This will simply return `0`s forever. The done flag is never set to true, so it will never stop returning 0s.

#### Exercise

Build an iterator that will return the numbers 1 through 10 in sequence, and then stop.
