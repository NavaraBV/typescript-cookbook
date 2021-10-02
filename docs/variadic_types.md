## Variadic types

Variadic generic types can support an arbitrary amount of parameters with an arbitrary type (it might be restricted, but it not necessarily needs to be).
For example, I want to create a function that accepts an arbitrary number of values which and return them as a strongly typed array of observables of that type.

It would look something like this:

```ts
type ArrayOfObservables<T extends any[]> = {
    [K in keyof T]: Observable<T[K]>;
};

const mapToObservable = <T extends any[]>(...args: T): ArrayOfObservables<T> => args.map(value => of(value)) as ArrayOfObservables<T>;

const value1: string = 'hello';
const value2: string[] = ['w', 'o', 'r', 'l', 'd'];
const value3: boolean = true;

const observableArray = mapToObservable(value1, value2, value3);
// : [Observable<string>, Observable<string[]>, Observable<boolean>]
```

### Using tuple types to get more flexibility

Ever seen a construct like this?:

```ts
// Function overloads to support strong typing
export function on<C1 extends ActionCreator, S>(creator1: C1, reducer: OnReducer<S, [C1]>): On<S>;
export function on<C1 extends ActionCreator, C2 extends ActionCreator, S>(
    creator1: C1,
    creator2: C2,
    reducer: OnReducer<S, [C1, C2]>
): On<S>;
export function on<C1 extends ActionCreator, C2 extends ActionCreator, C3 extends ActionCreator, S>(
    creator1: C1,
    creator2: C2,
    creator3: C3,
    reducer: OnReducer<S, [C1, C2, C3]>
): On<S>;

// ...
// Actual function implementation
export function on(...args: (ActionCreator | Function)[]): { reducer: Function; types: string[] } {
    const reducer = args.pop() as Function;
    const types = args.reduce((result, creator) => [...result, (creator as ActionCreator).type], [] as string[]);
    return { reducer, types };
}
```

JavaScript (and TypeScript for that matter) does not accept a spread operator as first argument.
The usecase here is to allow an arbitrary number of creators and then have one mapping function combining them.
Since TS did not allow that, you needed to work with these ugly overloads to explicitly allow for up to 10 generic arguments.

In the newest versions of TS starting from 4.1, you can make this work using only one spread operator and a generic type:
```ts
function doSomethingWithParams<T extends readonly any[], R>(...args: [...values: T, mapper: (...args: T) => R]): R {
    const mapper = args[args.length - 1];
    return mapper(args.slice(0, -1));
}

const mapped = doSomethingWithParams('1', 2, {someArray: [1,2,3]}, (str, num, obj) => 
    `${str} ${num} ${obj.someArray.join(',')}`);
);
```


Right now, the current implementation of `on` is this:

```ts
export function on<State, Creators extends readonly ActionCreator[]>(
    ...args: [...creators: Creators, reducer: OnReducer<State extends infer S ? S : never, Creators>]
): ReducerTypes<State, Creators> {
    const reducer = args.pop() as OnReducer<any, Creators>;
    const types = (args as unknown as Creators).map(creator => creator.type) as unknown as ExtractActionTypes<Creators>;
    return { reducer, types };
}
```

If you want to read more about variadic typing and want to implement a `curry` function (for example `add(x, y)` => `add(x) => add(y) => x + y`):

-   https://www.freecodecamp.org/news/typescript-curry-ramda-types-f747e99744ab/
