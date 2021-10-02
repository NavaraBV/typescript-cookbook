## Basic Types

### What is the difference between a function that has return type `void` and `never`

    1. `void` indicates that function will return no value, not even undefined.
    Where `never` indicates that the function will not have reachable end point.
    2. `void` can not be used as a return type of a function.
    3. `void` and `never` both indicate that no value will be returned, not even undefined.

Answer: 1.

### What is the difference between an `interface` and a `type`, except for the syntax difference?

    1. `type` support binary operators like `|` and `&`, where an `interface` can only be extended
    2. `interface` support declaration merging, where `type` would throw an error
    3. `type` can be used to rename primitives (`type UUID = string`), where an `interface` can't
    4. All of the above
    5. None of the above

Answer: 4.

### What can you use to make a value immutable

    1. Use an `immutable` library
    2. Declare the value as `readonly`, with all the properties and functions within.
    3. Use `as const`
    4. All above

Answer: 4.

### What is the result of the following statement: `const x = [23, '123', true]`?

1. `[number, string, boolean]`
2. `[23, '123', true]`
3. `(string | number | boolean)[]`
4. `('123' | 23 | true)[]`

## Variadic types

### How do you call this kind of typing construct: `const x = <T extends readonly any[]>(...args: T): T => {...}`?

    1. variadic function
    2. variadic generic function
    3. curry function
    4. template type function

Answer 2.
