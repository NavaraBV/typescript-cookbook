# Template Strings

With the introduction of [4.1](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-1.html) we got template strings, all thanks to PR [#40336](https://github.com/microsoft/TypeScript/pull/40336), which is really impressive work.

I personally think this will explode in the near future will all crazy kind of things.
Couple of examples already created:
* getting the correct type of `get(obj,'deeplvl1[1].deeplvl2.deeplvl3[88].deeplvl4.value')`
* Writing a JSON parser: https://twitter.com/buildsghost/status/1301976526603206657
* Converting a SQL statement to a typed result: https://github.com/codemix/ts-sql

## Examples

### Spring request
```ts
type SpringRequest<Entity> = {
    page: number;
    sort: `${keyof Omit<Entity, symbol>},${'asc' | 'desc'}`;
} & {
    [P in keyof Entity]: string;
};

class Person {
    public name!: string;
    public address!: string;
    filterAndSort(request: SpringRequest<this>) {
        // make call
    }
}
```

### Force protocol
```ts
type Protocol<T extends string, U extends string> = `${T}://${U}`;
function download(hostSpec: Protocol<"http" | "https" | "ftp", string>) { }
```

### Event listener
```ts
type FunctionPropertyNames<T> = {
    [K in keyof T]: T[K] extends Function ? K : never
}[keyof T];

type PropsOnly<Entity> = string & keyof Omit<Entity, FunctionPropertyNames<Entity>>;
class Person {
    public name!: string;
    public address!: string;

    on(event: `${PropsOnly<Person>}Change`) {
        // event is either nameChange or addressChange
    }
}
```

### Cypress-like autocompletion
```ts
type CypressTypes = `${'not.' | ''}be.${'visible' | 'enabled'}`;
```

### Query DSL
```ts
export type ExtractVars<Path extends string> =
    '' extends Path ? {} :
    Path extends `${string}#{${infer VarName}}${infer Rest}` ?
        Rest extends '' ? {[K in VarName]: number|string} : {[K in VarName]: number|string} & ExtractVars<Rest> :
    Path extends `${string}#{${infer VarName}}` ? {[K in VarName]: number|string} :
    Path extends `#{${infer VarName}}${infer Rest}` ? {[K in VarName]: number|string} & ExtractVars<Rest> : {};

declare function queryDSL<Path extends string>(s: Path, mapping: ExtractVars<Path>) : string;

// Your IDE will auto complete the second argument of queryDSL
queryDSL(`select #{column}, #{column2} from table where column=#{myVar}`, {
    column: 'column1',
    myVar: 2333,
    column2: 233,
});
```