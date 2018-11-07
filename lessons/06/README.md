# Writing to memory from Javascript

Now we have seen how to manipulate memory from inside WASM, but this is not much
better than just calling our conversion functions directly. So now we will see
how we can manipulate memory from outside.

As mentioned earlier, WASM has no concept of data structures, so we will need to
make our own. In Javascript an `Array` is quite a number of things at the same
time. It's resizeable, it supports queue operations like shift/unshift, it also
works like a stack with push/pop and we can have holes in the middle of the
array, we cannot go out of bounds easily (this will cause an error instead of
giving us garbage data), it can have heterogeneous data (numbers, strings,
objects, etc. in the same array) and we do not have to think about the
underlying memory.

We have seen that we can pretend the memory in WASM is an array if we fix up
the indexes and multiply them up to the data length. From Javascript we can
also manipulate the memory as if it was an Array of `f64`, namely with the
[TypedArray `Float64Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Float64Array).

This can be done like this:

```js
var wasm = require('./memory')

var mod = wasm()

// Change the view of the wasm memory to Float64
var f64Array = new Float64Array(mod.memory.buffer)

f64Array[0] = 1
f64Array[1] = 10e-100

console.log(mod.exports['f64.get'](0) === 1)
console.log(mod.exports['f64.get'](1) === 10e-100)
```

## Exercise

Write javascript that sends a `f64`array of size 3 to your WASM program, which
takes these three numbers, adds them, and returns the result back to the
javascript program. The javascript program should print out the result.

[**Exercise 07**](../07)
