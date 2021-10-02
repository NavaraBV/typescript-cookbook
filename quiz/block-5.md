## Utility types
### If you want to make all properties of an object not required, which type operator do you use?
    1. `Required`
    2. `Optional`
    3. `Nullable`
    4. `Partial`

Answer: 4.

### Which of the following constructs is valid?
```ts
    1. type Optional<T, K extends T> = Pick<Partial<T>, K> & Omit<T, K>;
    2. type Optional<T, K extends keyof T> = Pick<Partial<T>, K> & Omit<T, K>;
    3. type Optional<T, K extends keyof T> = Pick<K> & Omit<T, K>;
    4. type Optional<T, K extends keyof T> = Pick<Partial<T>, K> & Omit<T>;
```

Answer 2.

### What is the correct implementation of Record<Key,Value>.
   Which describes what the type of the key and values are of an object.
```ts
    1. type Record<K extends keyof any, T> = { [P in K]: T; };
    2. type Record<K, T> = { [P in keyof K]: T; };
    3. type Record<K, T> = { [P in K]: T; };
```
Answer: 1.

### How must you define `ClassType` so that the function `declare function initClass(clazz: ClassType): void` only accepts Typescript classes?

```ts
1. type ClassType = new (...args: any[]) => Class;
2. type ClassType = typeof Class;
3. type ClassType = new (...args: any[]) => any;
```

Answer: 3. (but it also accepts Function)

## Advanced generic typing

### What is the type of `value` in the following code snippet:
    ```ts
    function wrapper(fn: (...args: any[]) => any) {
        return fn('someinput');
    }
    const value = wrapper((input: string) => input.toLowerCase());
    ```
    1. `string`
    2. `any`
    3. `Function`

Answer: 2.

### What is the type of `value` in the following code snippet:
    ```ts
    function wrapper<FN extends (...args: any[]) => any>(fn: FN): ReturnType<FN> {
        return fn('someinput');
    }
    const value = wrapper((input: string) => input.toLowerCase());
    ```
    1. `string`
    2. `any`
    3. `Function`
Answer: 1.

### What is the type of `result` in the following code snippet:

    ```ts
    type ElementType<T> = T extends ReadonlyArray<infer U> ? ElementType<U> : T;

    declare function unknownFunction<T extends unknown[]>(arr: T): ElementType<T>[];

    const result = unknownFunction([1, 2, 34, [23, 42, [21]]]);
    ```

    1. None, this does not compile
    2. `(number | (number | number[])[])[]`
    3. `number[]`
    4. `number`

Answer: 3.

### What is the value of `What` in the following code snipper:

    ```ts
    type ExtractWhat<T> = {
        [K in keyof T]: T[K] extends (...args: any[]) => any ? K : never;
    }[keyof T];

    class Question {
        current!: number;
        next() {}
        previous() {}
    }

    type What = ExtractWhat<Question>;
    ```

    1. `["next", "previous"]`
    2. Does not compile
    3. `"current"`
    4. `"next" | "previous"`

Answer: 4.

### How do you write a generic that takes the tail of a list? For example:

    ```ts
    type result = Tail<[1, 2, 3, 4, 5]>; // [2,3,4,5]
    ```
```ts
    1. Lol what?
    2. type Tail<T> = T extends [any, ...infer U] ? U : [...T];
    3. type Tail<T extends readonly unknown[]> = T extends readonly [any, ...infer U] ? U : [...T];
    4. type Tail<T extends readonly any[]> = T extends readonly [_, ...infer U] ? U : [...T];
```

Answer: 3. (See also https://www.youtube.com/watch?v=IGw2MRI0YV8)

### How do you write a generic that takes the head of a list?
```ts
    1. type Head<T extends readonly unknown[]> = T[0];
    2. type Head<T extends readonly unknown[]> = T extends readonly [infer U] ? U : undefined;
    3. type Head<T extends readonly unknown[]> = T extends readonly [infer U, ...any] ? U : undefined;
```
Answer: 1.


### Open question: when do you use `any`
    Discussieren over any vs unknown