# Array.prototype.filter()

The `filter()` method **creates a new array** with all elements that pass the test implemented by the provided function.

## Syntax

    // Arrow function
    filter((element) => { ... } )
    filter((element, index) => { ... } )
    filter((element, index, array) => { ... } )

    // Callback function
    filter(callbackFn)
    filter(callbackFn, thisArg)

    // Inline callback function
    filter(function callbackFn(element) { ... })
    filter(function callbackFn(element, index) { ... })
    filter(function callbackFn(element, index, array){ ... })
    filter(function callbackFn(element, index, array) { ... }, thisArg)

### Parameters

`callbackFn`
Function is a predicate, to test each element of the array. Return a value that coerces to `true` to keep the element, or to `false` otherwise.

It accepts three arguments:

`element`
The current element being processed in the array.

`index`<span class="badge inline optional">Optional</span>
The index of the current element being processed in the array.

`array`<span class="badge inline optional">Optional</span>
The array `filter` was called upon.

`thisArg`<span class="badge inline optional">Optional</span>
Value to use as `this` when executing `callbackFn`.

### Return value

A new array with the elements that pass the test. If no elements pass the test, an empty array will be returned.

## Description

`filter()` calls a provided `callbackFn` function once for each element in an array, and constructs a new array of all the values for which `callbackFn` returns [a value that coerces to `true`](https://developer.mozilla.org/en-US/docs/Glossary/Truthy). `callbackFn` is invoked only for indexes of the array which have assigned values; it is not invoked for indexes which have been deleted or which have never been assigned values. Array elements which do not pass the `callbackFn` test are skipped, and are not included in the new array.

`callbackFn` is invoked with three arguments:

1.  the value of the element
2.  the index of the element
3.  the Array object being traversed

If a `thisArg` parameter is provided to `filter`, it will be used as the callback's `this` value. Otherwise, the value `undefined` will be used as its `this` value. The `this` value ultimately observable by `callback` is determined according to [the usual rules for determining the `this` seen by a function](../../operators/this).

`filter()` does not mutate the array on which it is called.

The range of elements processed by `filter()` is set before the first invocation of `callbackFn`. Elements which are appended to the array (from `callbackFn`) after the call to `filter()` begins will not be visited by `callbackFn`. If existing elements of the array are deleted in the same way they will not be visited.

## Polyfill

`filter()` was added to the ECMA-262 standard in the 5th edition. Therefore, it may not be present in all implementations of the standard.

You can work around this by inserting the following code at the beginning of your scripts, allowing use of `filter()` in ECMA-262 implementations which do not natively support it. This algorithm is exactly equivalent to the one specified in ECMA-262, 5th edition, assuming that `fn.call` evaluates to the original value of [`Function.prototype.bind()`](../function/bind), and that [`Array.prototype.push()`](push) has its original value.

    if (!Array.prototype.filter){
      Array.prototype.filter = function(func, thisArg) {
        'use strict';
        if ( ! ((typeof func === 'Function' || typeof func === 'function') && this) )
            throw new TypeError();

        var len = this.length >>> 0,
            res = new Array(len), // preallocate array
            t = this, c = 0, i = -1;

        var kValue;
        if (thisArg === undefined){
          while (++i !== len){
            // checks to see if the key was set
            if (i in this){
              kValue = t[i]; // in case t is changed in callback
              if (func(t[i], i, t)){
                res[c++] = kValue;
              }
            }
          }
        }
        else{
          while (++i !== len){
            // checks to see if the key was set
            if (i in this){
              kValue = t[i];
              if (func.call(thisArg, t[i], i, t)){
                res[c++] = kValue;
              }
            }
          }
        }

        res.length = c; // shrink down array to proper size
        return res;
      };
    }

## Examples

### Filtering out all small values

The following example uses `filter()` to create a filtered array that has all elements with values less than `10` removed.

    function isBigEnough(value) {
      return value >= 10
    }

    let filtered = [12, 5, 8, 130, 44].filter(isBigEnough)
    // filtered is [12, 130, 44]

### Find all prime numbers in an array

The following example returns all prime numbers in the array:

    const array = [-3, -2, -1, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13];

    function isPrime(num) {
      for (let i = 2; num > i; i++) {
        if (num % i == 0) {
          return false;
        }
      }
      return num > 1;
    }

    console.log(array.filter(isPrime)); // [2, 3, 5, 7, 11, 13]

### Filtering invalid entries from JSON

The following example uses `filter()` to create a filtered json of all elements with non-zero, numeric `id`.

    let arr = [
      { id: 15 },
      { id: -1 },
      { id: 0 },
      { id: 3 },
      { id: 12.2 },
      { },
      { id: null },
      { id: NaN },
      { id: 'undefined' }
    ]

    let invalidEntries = 0

    function filterByID(item) {
      if (Number.isFinite(item.id) && item.id !== 0) {
        return true
      }
      invalidEntries++
      return false;
    }

    let arrByID = arr.filter(filterByID)

    console.log('Filtered Array\n', arrByID)
    // Filtered Array
    // [{ id: 15 }, { id: -1 }, { id: 3 }, { id: 12.2 }]

    console.log('Number of Invalid Entries = ', invalidEntries)
    // Number of Invalid Entries = 5

### Searching in array

Following example uses `filter()` to filter array content based on search criteria.

    let fruits = ['apple', 'banana', 'grapes', 'mango', 'orange']

    /**
     * Filter array items based on search criteria (query)
     */
    function filterItems(arr, query) {
      return arr.filter(function(el) {
          return el.toLowerCase().indexOf(query.toLowerCase()) !== -1
      })
    }

    console.log(filterItems(fruits, 'ap'))  // ['apple', 'grapes']
    console.log(filterItems(fruits, 'an'))  // ['banana', 'mango', 'orange']

#### ES2015 Implementation

    const fruits = ['apple', 'banana', 'grapes', 'mango', 'orange']

    /**
     * Filter array items based on search criteria (query)
     */
    const filterItems = (arr, query) => {
      return arr.filter(el => el.toLowerCase().indexOf(query.toLowerCase()) !== -1)
    }

    console.log(filterItems(fruits, 'ap'))  // ['apple', 'grapes']
    console.log(filterItems(fruits, 'an'))  // ['banana', 'mango', 'orange']

### Affecting Initial Array (modifying, appending and deleting)

The following examples tests the behavior of the `filter` method when the array is modified.

    // Modifying each words
    let words = ['spray', 'limit', 'exuberant', 'destruction','elite', 'present']

    const modifiedWords = words.filter( (word, index, arr) => {
      arr[index+1] +=' extra'
      return word.length < 6
    })

    console.log(modifiedWords)
    // Notice there are three words below length 6, but since they've been modified one is returned
    // ["spray"]

    // Appending new words
    words = ['spray', 'limit', 'exuberant', 'destruction','elite', 'present']
    const appendedWords = words.filter( (word, index, arr) => {
      arr.push('new')
      return word.length < 6
    })

    console.log(appendedWords)
    // Only three fits the condition even though the `words` itself now has a lot more words with character length less than 6
    // ["spray" ,"limit" ,"elite"]

    // Deleting words
    words = ['spray', 'limit', 'exuberant', 'destruction', 'elite', 'present']
    const deleteWords = words.filter( (word, index, arr) => {
      arr.pop()
      return word.length < 6
    })

    console.log(deleteWords)
    // Notice 'elite' is not even obtained as its been popped off `words` before filter can even get there
    // ["spray" ,"limit"]

## Specifications

<table>
<thead>
<tr class="header">
<th>Specification</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>
<a href="https://tc39.es/ecma262/#sec-array.prototype.filter">ECMAScript Language Specification (ECMAScript)
<br/>

<span class="small">#sec-array.prototype.filter</span>
</a>
</td>
</tr>
</tbody>
</table>

`filter`

1

12

1.5

9

9.5

3

≤37

18

4

10.1

1

1.0

## See also

-   [`Array.prototype.forEach()`](foreach)
-   [`Array.prototype.every()`](every)
-   [`Array.prototype.some()`](some)
-   [`Array.prototype.reduce()`](reduce)
-   [`Array.prototype.find()`](find)

<a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter" class="_attribution-link">https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter</a>
