# Reflect.construct()

The static `Reflect.construct()` method acts like the [`new`](../../operators/new) operator, but as a function. It is equivalent to calling `new target(...args)`. It gives also the added option to specify a different prototype.

## Syntax

    Reflect.construct(target, argumentsList)
    Reflect.construct(target, argumentsList, newTarget)

### Parameters

`target`
The target function to call.

`argumentsList`
An array-like object specifying the arguments with which `target` should be called.

`newTarget` <span class="badge inline optional">Optional</span>
The constructor whose prototype should be used. See also the [`new.target`](../../operators/new.target) operator. If `newTarget` is not present, its value defaults to `target`.

### Return value

A new instance of `target` (or `newTarget`, if present), initialized by `target` as a constructor with the given `argumentsList`.

### Exceptions

A [`TypeError`](../typeerror), if `target` or `newTarget` are not constructors.

## Description

`Reflect.construct()` allows you to invoke a constructor with a variable number of arguments. (This would also be possible by using the [spread syntax](../../operators/spread_syntax) combined with the [`new` operator](../../operators/new).)

    let obj = new Foo(...args)
    let obj = Reflect.construct(Foo, args)

### `Reflect.construct()` vs `Object.create()`

Prior to the introduction of `Reflect`, objects could be constructed using an arbitrary combination of constructor and prototype by using [`Object.create()`](../object/create).

    function OneClass() {
        this.name = 'one'
    }

    function OtherClass() {
        this.name = 'other'
    }

    // Calling this:
    let obj1 = Reflect.construct(OneClass, args, OtherClass)

    // ...has the same result as this:
    let obj2 = Object.create(OtherClass.prototype)
    OneClass.apply(obj2, args)

    console.log(obj1.name)  // 'one'
    console.log(obj2.name)  // 'one'

    console.log(obj1 instanceof OneClass)  // false
    console.log(obj2 instanceof OneClass)  // false

    console.log(obj1 instanceof OtherClass)  // true
    console.log(obj2 instanceof OtherClass)  // true

    //Another example to demonstrate below:

    function func1(a, b, c, d) {
      console.log(arguments[3]);
    }

    function func2(d, e, f, g) {
      console.log(arguments[3]);
    }

    let obj1 = Reflect.construct(func1, ['I', 'Love', 'my', 'India'])
    obj1

However, while the end result is the same, there is one important difference in the process. When using `Object.create()` and [`Function.prototype.apply()`](../function/apply), the `new.target` operator will point to `undefined` within the function used as the constructor, since the `new` keyword is not being used to create the object.

When invoking `Reflect.construct()`, on the other hand, the `new.target` operator will point to the `newTarget` parameter if supplied, or `target` if not.

    function OneClass() {
        console.log('OneClass')
        console.log(new.target)
    }
    function OtherClass() {
        console.log('OtherClass')
        console.log(new.target)
    }

    let obj1 = Reflect.construct(OneClass, args)
    // Output:
    //     OneClass
    //     function OneClass { ... }

    let obj2 = Reflect.construct(OneClass, args, OtherClass)
    // Output:
    //     OneClass
    //     function OtherClass { ... }

    let obj3 = Object.create(OtherClass.prototype);
    OneClass.apply(obj3, args)
    // Output:
    //     OneClass
    //     undefined

## Examples

### Using `Reflect.construct()`

    let d = Reflect.construct(Date, [1776, 6, 4])
    d instanceof Date  // true
    d.getFullYear()    // 1776

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
<a href="https://tc39.es/ecma262/#sec-reflect.construct">ECMAScript Language Specification (ECMAScript)
<br/>

<span class="small">#sec-reflect.construct</span>
</a>
</td>
</tr>
</tbody>
</table>

`construct`

49

12

42

No

36

10

49

49

42

36

10

5.0

## See also

-   [`Reflect`](../reflect)
-   [`new`](../../operators/new)
-   `new.target`

© 2005–2021 MDN contributors.
Licensed under the Creative Commons Attribution-ShareAlike License v2.5 or later.
<a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect/construct" class="_attribution-link">https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect/construct</a>
