## Template literal types

###  What is the type result of the following statement:

```ts
type SomeType = `${'top' | 'bottom'}-${'right' | 'left'}`;

    1. Does not compile
    2. string
    3. `${'top' | 'bottom'}-${'right' | 'left'}`
    4. "top-right" | "top-left" | "bottom-right" | "bottom-left"
```
Answer: 4.

### What is the type result of `FourDigits` the following statement:

```ts
type Digit = 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9;
type FourDigits = `${Digit}${Digit}${Digit}${Digit}`;

    1. Does not compile: "Expression produces a union type that is too complex to represent"
    2. string
    3. `${Number}${Number}${Number}${Number}`
    4. "0000" | "0001" | ... | "9999"
```
Answer: 3.

###  What is the type result of `FiveDigits` the following statement:

```ts
type Digit = 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9;
type FiveDigits = `${Digit}${Digit}${Digit}${Digit}${Digit}`;

    1. Does not compile: "Expression produces a union type that is too complex to represent"
    2. string
    3. `${Number}${Number}${Number}${Number}${Number}`
    4. "0000" | "0001" | ... | "9999"
```
Answer: 1.