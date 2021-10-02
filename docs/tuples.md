## Tuples

```ts
let x: [string, number];
x = ['hello', 1];

console.log(x[0], x[1]); // 'hello', 1

x = [1, 'hello']; // Does not compile
```

You can also name tuples nowadays:

```ts
let x: [message: string, amount: number];
x = ['hello', 1];
console.log(x[0], x[1]); // 'hello', 1
```

Typescript usually does not infer tuples from intermediate types:

```ts
of('World')
    .pipe(map(name => [`Hello, ${name}!`, 23]))
    .subscribe(console.log); // ["Hello, World!", 23]
// Type of things function:
// Observable<(string | number)[]>.subscribe(next: (value: (string | number)[]) => void)
```

If we then want to use it as it were a tuple, it does not work:

```ts
of('World')
    .pipe(map(name => [`Hello, ${name}!`, 23]))
    .subscribe(([name, amount]) => 
        // Property 'toUpperCase' does not exist on type 'string | number'
        console.log(name.toUpperCase(), amount));
```

To fix that, we can either specify the return type by hand:

```ts
of('World')
    .pipe(map((name): [string, number] => [`Hello, ${name}!`, 23]))
    .subscribe(([name, amount]) => console.log(name.toUpperCase(), amount)); // works like a charm
```

Or my personal favorite, use `as const` to let TS know that we provide these values as a constant array (aka tuple):
```ts
of('World')
    .pipe(map(name => [`Hello, ${name}!`, 23] as const))
    .subscribe(([name, amount]) => console.log(name.toUpperCase(), amount)); // works like a charm
```

