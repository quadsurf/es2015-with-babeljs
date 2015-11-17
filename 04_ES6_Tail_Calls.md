# ES6 Tail Calls

## Objectives

* Describe what a tail call is and explain what triggers a tail call optimization

## Tail Calls

Tail calls are an optimization added to most functional-esque languages that allow for recursion without hitting call stack limits. A tail call happens whenever a function calls another function as it's last action. Languages that have implemented tail call optimizations don't need to keep the existing function call on the stack anymore, they're able to essentially just replace the currently executing frame in the stack with the new function call. This ensures that you don't blow up the stack when recursing heavily.

Try the following code in your browser:

```javascript
function factorial(n, acc) {
  'use strict';
  if (typeof(acc) === 'undefined') {
    acc = 1;
  }
  if (n <= 1) {
    return acc;
  }
  return factorial(n - 1, n * acc);
}

factorial(100000);
```

It will most likely hit a call stack limit (unless you're going through this curriculum after your browser has implemented these es2015 optimizations!) As of the time of this writing, these optimizations are not implemented in any of the released versions of node either. Babel can do some magic to transpile this into something that will work, but to do that we have to use a default argument for acc as well:

```javascript
function factorial(n, acc = 1) {
  'use strict';
  if (n <= 1) {
    return acc;
  }
  return factorial(n - 1, n * acc);
}
```

If you run that through the [online babel translator](https://babeljs.io/repl/#?experimental=false&evaluate=true&loose=false&spec=false&code=function%20factorial(n%2C%20acc%20%3D%201)%20%7B%0A%20%20'use%20strict'%3B%0A%20%20if%20(n%20%3C%3D%201)%20%7B%0A%20%20%20%20return%20acc%3B%0A%20%20%7D%0A%20%20return%20factorial(n%20-%201%2C%20n%20*%20acc)%3B%0A%7D) you'll get code that looks something like this:

```javascript
'use strict';

function factorial(_x2) {
  var _arguments = arguments;
  var _again = true;

  _function: while (_again) {
    var n = _x2;

    'use strict';
    _again = false;
    var acc = _arguments.length <= 1 || _arguments[1] === undefined ? 1 : _arguments[1];
    if (n <= 1) {
      return acc;
    }
    _arguments = [_x2 = n - 1, n * acc];
    _again = true;
    acc = undefined;
    continue _function;
  }
}
```

Which will, essentially, just run a while loop until the factorial has been determined. It's important to note that it can _only_ optimize recursive functions that end with a single function call. This means our standard fibonacci example:

```javascript
function fib(val) {
  'use strict';
  if (val == 0) {
    return 0;
  }
  if (val == 1) {
    return 1;
  }
  return fib(val - 1) + fib(val - 2);
}
```

Can not be optimized. This expands out to multiple function calls, so there's no way to simply replace the currently executing frame with a single new one.

#### Exercise

Write out, in your own words, what triggers tail call optimization and describe what a tail call is.
