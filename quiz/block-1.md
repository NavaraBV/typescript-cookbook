## TSConfig
###  What does the `path` option do in your `tsconfig.json`

    1. Tells to compiler where it can find other `tsconfigs`
    2. Specifies where typescript can find your `node_modules`
    3. Allows you specify how typescript should resolve an import

Answer: 3.

### Can you import `json` files directly in a `ts` file?

    1. Yes, but only with a configuration flag
    2. Yes, out of the box
    3. No, you would need something like webpack to resolve `json` files

Answer: 1.

## Compiler output

###  What is the output of compiling the following piece of typescript code:

```ts
function test<T extends {}>(input: T) {
    console.log(input);
}

test('123');
```

```js
// 1
Does not compile

// 2
'use strict';
function test(input) {
    console.log(input);
}
test('123');

// 3
'use strict';
function test(input: string) {
    console.log(input);
}
test('123');
```

Answer: 2.

### What is the result of compiling the following ts code:

```ts
class X {
    private f: boolean = false;

    public changeF(): void {
        this.f = !this.f;
    }
}

const x = new X();
x.changeF();
console.log(x.f);
```

    1. Does not compile
    2. It compiles, but throws an error on runtime
    3. It just prints out `true`
    4. It prints out `undefined`
Answer: 1.

### What is the result of compiling the following ts code:

```ts
class X {
    private f: boolean = false;

    public changeF(): void {
        this.f = !this.f;
    }
}

const x = new X();
x.changeF();
console.log((x as any).f);
```

    1. Does not compile
    2. It compiles, but throws an error on runtime
    3. It just prints out `true`
    4. It prints out `undefined`

Answer: 3.

### Targeting ES2015, What is the result of writing the following code:

```ts
class X {
    #f: boolean = false;

    public changeF(): void {
        this.#f = !this.#f;
    }
}

const x = new X();
x.changeF();
console.log((x as any).f);
```

    1. Does not compile
    2. It compiles, but throws an error on runtime
    3. It just prints out `true`
    4. It prints out `undefined`

Answer: 4.

## Tooling
### How do you run a typescript file?

    1. Using `tsc`
    2. You can run it directly with `ts-node`
    3. You can directly use `node file.ts`

Answer: 2.