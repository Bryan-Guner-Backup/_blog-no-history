# Number.MAX_SAFE_INTEGER

The `Number.MAX_SAFE_INTEGER` constant represents the maximum safe integer in JavaScript (`253 - 1`).

For larger integers, consider using [`BigInt`](../bigint).

Property attributes of `Number.MAX_SAFE_INTEGER`

Writable

no

Enumerable

no

Configurable

no

## Description

The `MAX_SAFE_INTEGER` constant has a value of `9007199254740991` (9,007,199,254,740,991 or ~9 quadrillion). The reasoning behind that number is that JavaScript uses [double-precision floating-point format numbers](https://en.wikipedia.org/wiki/Double_precision_floating-point_format) as specified in [IEEE 754](https://en.wikipedia.org/wiki/IEEE_floating_point) and can only safely represent integers between `-(253 - 1)` and `253 - 1`.

Safe in this context refers to the ability to represent integers exactly and to correctly compare them. For example, `Number.MAX_SAFE_INTEGER + 1 === Number.MAX_SAFE_INTEGER + 2` will evaluate to true, which is mathematically incorrect. See [`Number.isSafeInteger()`](issafeinteger) for more information.

This field does not exist in old browsers. Using it without checking its existence, such as `Math.max(Number.MAX_SAFE_INTEGER, 2)`, will yield undesired results such as NaN.

Because `MAX_SAFE_INTEGER` is a static property of [`Number`](../number), you always use it as `Number.MAX_SAFE_INTEGER`, rather than as a property of a [`Number`](../number) object you created.

## Polyfill

    if (!Number.MAX_SAFE_INTEGER) {
        Number.MAX_SAFE_INTEGER = 9007199254740991; // Math.pow(2, 53) - 1;
    }

## Examples

### Return value of MAX_SAFE_INTEGER

    Number.MAX_SAFE_INTEGER; // 9007199254740991

### Numbers higher than safe integer

This returns 2 because in floating points, the value is actually the decimal trailing "1" except for in subnormal precision cases such as zero.

    Number.MAX_SAFE_INTEGER * Number.EPSILON; // 2

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
<a href="https://tc39.es/ecma262/#sec-number.max_safe_integer">ECMAScript Language Specification (ECMAScript)
<br/>

<span class="small">#sec-number.max_safe_integer</span>
</a>
</td>
</tr>
</tbody>
</table>

`MAX_SAFE_INTEGER`

34

12

31

No

21

9

≤37

34

31

21

9

2.0

## See also

-   [`Number.MIN_SAFE_INTEGER`](min_safe_integer)
-   [`Number.isSafeInteger()`](issafeinteger)
-   [`BigInt`](../bigint)

© 2005–2021 MDN contributors.
Licensed under the Creative Commons Attribution-ShareAlike License v2.5 or later.
<a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/MAX_SAFE_INTEGER" class="_attribution-link">https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/MAX_SAFE_INTEGER</a>
