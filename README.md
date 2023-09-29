# Decyphering `/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/`

A regex, which is short for regular expression, is a sequence of characters that defines a specific search pattern. When included in code or search algorithms, regular expressions can be used to find certain patterns of characters within a string, or to find and replace a character or sequence of characters within a string. They are also frequently used to validate input.

## Summary

For example, the following regular expression can be used to verify that user input is a valid email address:

`/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/`

Each component of this regex has a unique responsibility to make sure that a user enters an email address that begins with an unspecified number of characters preceding the @ symbol, followed by a domain.

## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [OR Operator](#or-operator)
- [Character Classes](#character-classes)
- [Flags](#flags)
- [Grouping and Capturing](#grouping-and-capturing)
- [Bracket Expressions](#bracket-expressions)
- [Greedy and Lazy Match](#greedy-and-lazy-match)
- [Boundaries](#boundaries)
- [Back-references](#back-references)
- [Look-ahead and Look-behind](#look-ahead-and-look-behind)

## Regex Components

### Anchors
Anchors have a special meaning in regular expressions. They do not match any character. Instead, they match a position before or after characters. The ^ character asserts the beginning of a string, and the $ character asserts the end.

### Quantifiers
Quantifiers denote the number of instances of a character. the expression `/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/` has multiple quantifiers within it. The `+` quantifier appears twice and means the characters before it can appear more than once. The last part of this regex,` ([a-z\.]{2,6})` has the quantifier, {2,6} meaning the characters `a-z\.` can only be used between 2 and 6 times.

### OR Operator
The regex for an email address does not feature the `|` or operator, but I'll give an example that does. The following regex can be used to match a Hex value:

`/^#?([a-f0-9]{6}|[a-f0-9]{3})$/`

This regex features two expressions separated by the or operator, `|` which allows you to search for either valid option.

### Character Classes
Character Classes are used to distinguish between different numbers, letters, and symbols.

`[a-z]` allows all letters in lowercase from a to z.

`[0-9]` allows digits from 0-9.

`\d` matches a single character that is a digit.

`\.` matches a single period.

### Flags
Regular expressions have optional flags that allow for functionality like global searching and case-insensitive searching. These flags can be used separately or together in any order, and are included as part of the regular expression.

| Flag| Description | # Corresponding property |
|-----|:-----------:|-------------------------:|
| d   |  Generate indices for substring matches. | hasIndices |
| g   |  Global search. | global |
| i   |  Case-insensitive search. | ignoreCase |
| m   |  Allows ^ and $ to match newline characters. | multiline |
| s   |  Allows . to match newline characters. | dotAll |
| u   |  "Unicode"; treat a pattern as a sequence of Unicode code points. | unicode |
| v   |  An upgrade to the u mode with more Unicode features. | unicodeSets |
| y   |  Perform a "sticky" search that matches starting at the current position in the target string. | sticky |

Flag information sourced from https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions

### Grouping and Capturing
Parenthesis () create different groupings.

`/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/`

In the example, the first grouping includes

`([a-z0-9_\.-]+)`

this grouping includes a bracket expression and a quantifier

the second

`([\da-z\.-]+)`

also includes a bracket expression and a quantifier

and then

`([a-z\.]{2,6})`

this too includes bracket expression with the {2,6} quantifier.

Notice that the `^` and `$` anchors are on either end of the grouping constructs. Also note that the `@` character is not included in any grouping. Neither is the `.` in `\.`. The expression wants to match these characters exactly.

### Bracket Expressions
The brackets `[]` create a bracket expression that require a match to the list of expressions contained in the brackets.

`/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/`

in the expression above we see `[a-z0-9_\.-]` matches any letter a-z, is case sensitive, matches a character 0-9, and matches the characters "_" , "-" , and ".".

`[\da-z\.-]`

matches a single digit from 0-9, any character a-z (case sensitive), and the characters "." and "-".

and

`[a-z\.]`

matches any character a-z(case sensitive) and the character ".".

### Greedy and Lazy Match
From https://stackoverflow.com/questions/2301285/what-do-lazy-and-greedy-mean-in-the-context-of-regular-expressions

'Greedy' means match longest possible string.

'Lazy' means match shortest possible string.

For example, the greedy h.+l matches 'hell' in 'hello' but the lazy h.+?l matches 'hel'.

In regards to our example regex `/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/` each grouping includes a greedy quantifier. For the first two groupings, `+` matches the previous token between one and unlimited times, as many times as possible, giving back as needed (greedy). For the final grouping, `{2,6}` matches the previous token between 2 and 6 times, as many times as possible, giving back as needed (greedy).

If we changed the first grouping to `([a-z0-9_\.-]+?)` we have a lazy match. This is because `+?` matches the previous token between one and unlimited times, as few times as possible, expanding as needed (lazy).

### Boundaries
The metacharacter `\b` is an anchor like the caret and the dollar sign. It matches at a position that is called a “word boundary”. This match is zero-length.

There are three different positions that qualify as word boundaries:

* Before the first character in the string, if the first character is a word character.
* After the last character in the string, if the last character is a word character.
* Between two characters in the string, where one is a word character and the other is not a word character.

Simply put: `\b` allows you to perform a “whole words only” search using a regular expression in the form of `\bword\b`.

Source: https://www.regular-expressions.info/wordboundaries.html

### Back-references
Backreferencing allows the results of a capture to be reused in later code. If `/1`, `/2`, `/3` etc are placed at the end of a grouping, the defined pattern in groups 1, 2, 3, or anything up to 99, will be repeated. 

An example without backreferencing:
```
let str = `He said: "She's the one!".`;

let regexp = /['"](.*?)['"]/g;

// The result is not what we'd like to have
alert( str.match(regexp) ); // "She'
```

Here it is with backreferencing:
```
let str = `He said: "She's the one!".`;

let regexp = /(['"])(.*?)\1/g;

alert( str.match(regexp) ); // "She's the one!"
```
### Look-ahead and Look-behind
Look-ahead and look-behind assertions (?=...) and (?<=...) allow us to match text based on what comes before or after it:

Positive Look-ahead (?=...) checks if a certain pattern is followed by another pattern. For example, foo(?=bar) matches 'foo' only when it's followed by 'bar'.

Positive Look-behind (?<=...) checks if a certain pattern is preceded by another pattern. For example, (?<=foo)bar matches 'bar' only when it's preceded by 'foo'.

## Author
[Jake Krauskopf](https://github.com/JaKrau)

A UC Berkley Full Stack Web Development Bootcamp (Week 17) Challenge.
