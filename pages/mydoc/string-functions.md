---
title: String functions
sidebar: mydoc_sidebar
permalink: string-functions.html
folder: mydoc
---

## `$string(arg)`

Casts the `arg` parameter to a string using the following casting rules

   - Strings are unchanged
   - Functions are converted to an empty string
   - Numeric infinity and NaN throw an error because they cannot be represented as a JSON number
   - All other values are converted to a JSON string using the [JSON.stringify](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) function

If `arg` is not specified (i.e. this function is invoked with no arguments), then the context value is used as the value of `arg`.

__Examples__

`$string(5)` => `"5"`  
`[1..5].$string()` => `["1", "2", "3", "4", "5"]`

## `$length(str)`

Returns the number of characters in the string `str`.  If `str` is not specified (i.e. this function is invoked with no arguments), then the context value is used as the value of `str`.  An error is thrown if `str` is not a string.

__Examples__

`$length("Hello World")` => `11`

## `$substring(str, start[, length])`

Returns a string containing the characters in the first parameter `str` starting at position `start` (zero-offset).  If `str` is not specified (i.e. this function is invoked with only the numeric argument(s)), then the context value is used as the value of `str`.  An error is thrown if `str` is not a string.

If `length` is specified, then the substring will contain maximum `length` characters.  

If `start` is negative then it indicates the number of characters from the end of `str`.  See [substr](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/substr) for full definition.

__Examples__

`$substring("Hello World", 3)` => `"lo World"`  
`$substring("Hello World", 3, 5)` => `"lo Wo"`  
`$substring("Hello World", -4)` => `"orld"`  
`$substring("Hello World", -4, 2)` => `"or"`

## `$substringBefore(str, chars)`

Returns the substring before the first occurrence of the character sequence `chars` in `str`.  If `str` is not specified (i.e. this function is invoked with only one argument), then the context value is used as the value of `str`.  If `str` does not contain `chars`, then it returns `str`.   An error is thrown if `str` and `chars` are not strings.

__Examples__

`$substringBefore("Hello World", " ")` => `"Hello"`

## `$substringAfter(str, chars)`

Returns the substring after the first occurrence of the character sequence `chars` in `str`.  If `str` is not specified (i.e. this function is invoked with only one argument), then the context value is used as the value of `str`.  If `str` does not contain `chars`, then it returns `str`.   An error is thrown if `str` and `chars` are not strings.

__Examples__

`$substringAfter("Hello World", " ")` => `"World"`


## `$uppercase(str)`

Returns a string with all the characters of `str` converted to uppercase.  If `str` is not specified (i.e. this function is invoked with no arguments), then the context value is used as the value of `str`.  An error is thrown if `str` is not a string.

__Examples__

`$uppercase("Hello World")` => `"HELLO WORLD"`


## `$lowercase(str)`

Returns a string with all the characters of `str` converted to lowercase.  If `str` is not specified (i.e. this function is invoked with no arguments), then the context value is used as the value of `str`.  An error is thrown if `str` is not a string.

__Examples__

`$lowercase("Hello World")` => `"hello world"`

## `$trim(str)`

Normalizes and trims all whitespace characters in `str` by applying the following steps:

- All tabs, carriage returns, and line feeds are replaced with spaces.
- Contiguous sequences of spaces are reduced to a single space.
- Trailing and leading spaces are removed.

If `str` is not specified (i.e. this function is invoked with no arguments), then the context value is used as the value of `str`.  An error is thrown if `str` is not a string.

__Examples__

`$trim("   Hello    \n World  ")` => `"Hello World"`


## `$pad(str, width [, char])`

Returns a copy of the string `str` with extra padding, if necessary, so that its total number of characters is at least the absolute value of the `width` parameter.  If `width` is a positive number, then the string is padded to the right; if negative, it is padded to the left.  The optional `char` argument specifies the padding character(s) to use.  If not specified, it defaults to the space character.

__Examples__

`$pad("foo", 5)` => `"foo  "`
`$pad("foo", -5)` => `"  foo"`
`$pad("foo", -5, "#")` => `"##foo"`
`$formatBase(35, 2) ~> $pad(-8, '0')` => `"00100011"`


## `$contains(str, pattern)`

Returns `true` if `str` is matched by `pattern`, otherwise it returns `false`. If `str` is not specified (i.e. this function is invoked with one argument), then the context value is used as the value of `str`.

The `pattern` parameter can either be a string or a regular expression (regex).  If it is a string, the function returns `true` if the characters within `pattern` are contained contiguously within `str`.  If it is a regex, the function will return `true` if the regex matches the contents of `str`.

__Examples__

`$contains("abracadabra", "bra")` => `true`  
`$contains("abracadabra", /a.*a/)` => `true`  
`$contains("abracadabra", /ar.*a/)` => `false`  
`$contains("Hello World", /wo/)` => `false`  
`$contains("Hello World", /wo/i)` => `true`  
`Phone[$contains(number, /^077/)]` => `{ "type": "mobile", "number": "077 7700 1234" }`

## `$split(str, separator [, limit])`

Splits the `str` parameter into an array of substrings.  If `str` is not specified, then the context value is used as the value of `str`.  It is an error if `str` is not a string.  

The `separator` parameter can either be a string or a regular expression (regex).  If it is a string, it specifies the characters within `str` about which it should be split.  If it is the empty string, `str` will be split into an array of single characters.  If it is a regex, it splits the string around any sequence of characters that match the regex.

The optional `limit` parameter is a number that specifies the maximum number of substrings to  include in the resultant array.  Any additional substrings are discarded.  If `limit` is not  specified, then `str` is fully split with no limit to the size of the resultant array.  It is an error if `limit` is not a non-negative number.

__Examples__

`$split("so many words", " ")` => `[ "so", "many", "words" ]`  
`$split("so many words", " ", 2)` => `[ "so", "many" ]`  
`$split("too much, punctuation. hard; to read", /[ ,.;]+/)` => `["too", "much", "punctuation", "hard", "to", "read"]`

## `$join(array[, separator])`

Joins an array of component strings into a single concatenated string with each component string separated by the optional `separator` parameter.

It is an error if the input array contains an item which isn't a string.

If `separator` is not specified, then it is assumed to be the empty string, i.e. no separator between the component strings.  It is an error if `separator` is not a string.

__Examples__

`$join(['a','b','c'])` => `"abc"`  
`$split("too much, punctuation. hard; to read", /[ ,.;]+/, 3) ~> $join(', ')` => `"too, much, punctuation"`

## `$match(str, pattern [, limit])`

Applies the `str` string to the `pattern` regular expression and returns an array of objects, with each object containing information about each occurrence of a match withing `str`.

The object contains the following fields:

- `match` - the substring that was matched by the regex.
- `index` - the offset (starting at zero) within `str` of this match.
- `groups` - if the regex contains capturing groups (parentheses), this contains an array of strings representing each captured group.

If `str` is not specified, then the context value is used as the value of `str`.  It is an error if `str` is not a string.

__Examples__

`$match("ababbabbcc",/a(b+)/)` =>
```
[
  {
    "match": "ab",
    "index": 0,
    "groups": ["b"]
  },
  {
    "match": "abb",
    "index": 2,
    "groups": ["bb"]
  },
  {
    "match": "abb",
    "index": 5,
    "groups": ["bb" ]
  }
]
```

## `$replace(str, pattern, replacement [, limit])`

Finds occurrences of `pattern` within `str` and replaces them with `replacement`.

If `str` is not specified, then the context value is used as the value of `str`.  It is an error if `str` is not a string.

The `pattern` parameter can either be a string or a regular expression (regex).  If it is a string, it specifies the substring(s) within `str` which should be replaced.  If it is a regex, its is used to find .

The `replacement` parameter can either be a string or a function.  If it is a string, it specifies the sequence of characters that replace the substring(s) that are matched by `pattern`.  If `pattern` is a regex, then the `replacement` string can refer to the characters that were matched by the regex as well as any of the captured groups using a `S` followed by a number `N`:

- If `N = 0`, then it is replaced by substring matched by the regex as a whole.
- If `N > 0`, then it is replaced by the substring captured by the Nth parenthesised group in the regex.
- If `N` is greater than the number of captured groups, then it is replaced by the empty string.
- A literal `$` character must be written as `$$` in the `replacement` string

If the `replacement` parameter is a function, then it is invoked for each match occurrence of the `pattern` regex.  The `replacement` function must take a single parameter which will be the object structure of a regex match as described in the `$match` function; and must return a string.

The optional `limit` parameter,  is a number that specifies the maximum number of replacements to make before stopping.  The remainder of the input beyond this limit will be copied to the output unchanged.

__Examples__

`$replace("John Smith and John Jones", "John", "Mr")` => `"Mr Smith and Mr Jones"`  
`$replace("John Smith and John Jones", "John", "Mr", 1)` => `"Mr Smith and John Jones"`  
`$replace("abracadabra", /a.*?a/, "*")` => `"*c*bra"`  
`$replace("John Smith", /(\w+)\s(\w+)/, "$2, $1")` => `"Smith, John"`  
`$replace("265USD", /([0-9]+)USD/, "$$$1")` => `"$265"`  
```
(
  $convert := function($m) { ($number($m.groups[0]) - 32) * 5/9 & "C" };
  $replace("temperature = 68F today", /(\d+)F/, $convert)
)
```
=> `"temperature = 20C today"`


## `$now()`

Generates a UTC timestamp in ISO 8601 compatible format and returns it as a string.  All invocations of `$now()` within an evaluation of an expression will all return the same timestamp value

__Examples__

`$now()` => `"2017-05-15T15:12:59.152Z"`


## `$fromMillis(number)`

Convert a number representing milliseconds since the Unix *Epoch* (1 January, 1970 UTC) to a timestamp string in the [ISO 8601](https://www.iso.org/iso-8601-date-and-time-format.html) format.

__Examples__

`$fromMillis(1510067557121)` => `"2017-11-07T15:12:37.121Z"`


## `$formatNumber(number, picture [, options])`

Casts the `number` to a string and formats it to a decimal representation as specified by the `picture` string.

The behaviour of this function is consistent with the XPath/XQuery function [fn:format-number](https://www.w3.org/TR/xpath-functions-31/#func-format-number) as defined in the XPath F&O 3.1 specification.  The picture string parameter defines how the number is formatted and has the [same syntax](https://www.w3.org/TR/xpath-functions-31/#syntax-of-picture-string) as fn:format-number.

The optional third argument `options` is used to override the default locale specific formatting characters such as the decimal separator.  If supplied, this argument must be an object containing name/value pairs specified in the [decimal format](https://www.w3.org/TR/xpath-functions-31/#defining-decimal-format) section of the XPath F&O 3.1 specification.

__Examples__

`$formatNumber(12345.6, '#,###.00')` => `"12,345.60"`
`$formatNumber(1234.5678, "00.000e0")` => `"12.346e2"`
`$formatNumber(34.555, "#0.00;(#0.00)")` => `"34.56"`
`$formatNumber(-34.555, "#0.00;(#0.00)")` => `"(34.56)"`
`$formatNumber(0.14, "01%")` => `"14%"`
`$formatNumber(0.14, "###pm", {"per-mille": "pm"})` => `"140pm"`
`$formatNumber(1234.5678, "①①.①①①e①", {"zero-digit": "\u245f"})` => `"①②.③④⑥e②"`


## `$formatBase(number [, radix])`

Casts the `number` to a string and formats it to an integer represented in the number base specified by the `radix` argument.  If `radix` is not specified, then it defaults to base 10.  `radix` can be between 2 and 36, otherwise an error is thrown.

`$formatBase(100, 2)` => "1100100"
`$formatBase(2555, 16)` => `"9fb"`


## `$base64encode()`

Converts an ASCII string to a base 64 representation. Each each character in the string is treated as a byte of binary data. This requires that all characters in the string are in the 0x00 to 0xFF range, which includes all characters in URI encoded strings. Unicode characters outside of that range are not supported.

__Examples__  

`$base64encode("myuser:mypass")` => `"bXl1c2VyOm15cGFzcw=="`


## `$base64decode()`

Converts base 64 encoded bytes to a string, using a UTF-8 Unicode codepage.

__Examples__  

`$base64decode("bXl1c2VyOm15cGFzcw==")` => `"myuser:mypass"`
