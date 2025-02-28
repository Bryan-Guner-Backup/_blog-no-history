# String.prototype.substr()

The `substr()` method returns a portion of the string, starting at the specified index and extending for a given number of characters afterwards.

## Syntax

    substr(start)
    substr(start, length)

### Parameters

`start`
The index of the first character to include in the returned substring.

`length`
Optional. The number of characters to extract.

### Return value

A new string containing the specified part of the given string.

## Description

`substr()` extracts `length` characters from a `str`, counting from the `start` index.

-   If `start` is a positive number, the index starts counting at the start of the string. Its value is capped at `str.length`.
-   If `start` is a negative number, the index starts counting from the end of the string. Its value is capped at `-str.length`.
-   Note: In Microsoft JScript, negative values of the `start` argument are not considered to refer to the end of the string.
-   If `length` is omitted, `substr()` extracts characters to the end of the string.
-   If `length` is [`undefined`](../undefined), `substr()` extracts characters to the end of the string.
-   If `length` is a negative number, it is treated as `0`.
-   For both `start` and `length`, [`NaN`](../nan) is treated as `0`.

## Polyfill

Microsoft's JScript does not support negative values for the start index. To use this feature in JScript, you can use the following code:

    // only run when the substr() function is broken
    if ('ab'.substr(-1) != 'b') {
      /**
       *  Get the substring of a string
       *  @param  {integer}  start   where to start the substring
       *  @param  {integer}  length  how many characters to return
       *  @return {string}
       */
      String.prototype.substr = function(substr) {
        return function(start, length) {
          // call the original method
          return substr.call(this,
            // did we get a negative start, calculate how much it is from the beginning of the string
            // adjust the start parameter for negative value
            start < 0 ? this.length + start : start,
            length)
        }
      }(String.prototype.substr);
    }

## Examples

### Using substr()

    var aString = 'Mozilla';

    console.log(aString.substr(0, 1));   // 'M'
    console.log(aString.substr(1, 0));   // ''
    console.log(aString.substr(-1, 1));  // 'a'
    console.log(aString.substr(1, -1));  // ''
    console.log(aString.substr(-3));     // 'lla'
    console.log(aString.substr(1));      // 'ozilla'
    console.log(aString.substr(-20, 2)); // 'Mo'
    console.log(aString.substr(20, 2));  // ''

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
<a href="https://tc39.es/ecma262/#sec-string.prototype.substr">ECMAScript Language Specification (ECMAScript)
<br/>

<span class="small">#sec-string.prototype.substr</span>
</a>
</td>
</tr>
</tbody>
</table>

## Browser compatibility

Desktop

Mobile

Chrome

Edge

Firefox

Internet Explorer

Opera

Safari

WebView Android

Chrome Android

Firefox for Android

Opera Android

Safari on IOS

Samsung Internet

`substr`

1

12

1

4

4

1

1

18

4

10.1

1

1.0

## See also

-   [`String.prototype.slice()`](slice)
-   [`String.prototype.substring()`](substring)

© 2005–2021 MDN contributors.
Licensed under the Creative Commons Attribution-ShareAlike License v2.5 or later.
<a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/substr" class="_attribution-link">https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/substr</a>
