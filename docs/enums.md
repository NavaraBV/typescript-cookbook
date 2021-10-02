## Enums

You can use enums with or without backing values.
If an enum is used to send values over to a backend, usually a string backing value is preferred.

```ts
export enum CarType {
    SMALL = 'small',
    HATCHBACK = 'hatchback',
    SUV = 'suv',
    PLUGIN_HYBRID = 'plugin-hybrid',
}

export interface Car {
    type: CarType;
}

export const suvCar: Car = {
    type: CarType.SUV,
};
```

To make sure that you are using the correct values and to have strong typing for enum values:

```ts
export const enumValueChecker =
    <Enum extends Record<string, string>, Key extends keyof Enum, Value extends Enum[Key]>(enumType: Enum) =>
    (value: string): value is Value =>
        !!Object.values(enumType).find(v => v === value);
```

Test file to complete it

```ts
describe('enumValueChecker', () => {
    enum TestEnum {
        X = 'X',
        Y = 'Y',
    }

    const testTypeGuard = (value: string): TestEnum | undefined => {
        if (enumValueChecker(TestEnum)(value)) {
            return value;
        } else {
            // cannot compile:
            // return value
            return undefined;
        }
    };

    it('should return typeguard for given enum', () => {
        expect(testTypeGuard('X')).toBeDefined();
        expect(testTypeGuard('Z')).toBeUndefined();
    });
});
```

## Enum assignability vs ducktyping

In TS we can make use of ducktyping, e.g. if an object has all the properties it needs to be assigned to some other type, regardless of it's own type, TS is fine with it.
In practice, if you have an `enum` with certain values which is defined in multiple places, assignability becomes an issue:
```ts
namespace N1 {
    export enum X {
        A = 'A',
        B = 'B',
        C = 'C'
    }
}
namespace N2 {
    export enum X {
        A = 'NOT_A',
        B = 'NOT_B',
        C = 'NOT_C'
    }
}

const p: N1.X = N2.X.A as N2.X; 
// NB cast added to make this example shorter - this works out of the box using an `interface` property for example.
```

Or:

```ts
// In package A
enum X {
    A = 'A',
    B = 'B',
    C = 'C',
}
interface A {
    property: X;
}

// In package B
enum Y {
    A = 'A',
    B = 'B',
    C = 'C',
}
interface B {
    property: Y;
}

// in another file:
import { X, A } from 'a';
import { B } from 'b';

const x: A = { property: X.A };
const y: B = x; // FAILS: enum X is not assignable to enum Y. the values are the same, but that's just not how enums work.
```

This example actually does work:

```ts
// In package A
enum X {
    A = 'A',
    B = 'B',
    C = 'C',
}
interface A {
    property: X;
}

// In package B
enum X {
    A = 'A',
    B = 'B',
    C = 'C',
}
interface B {
    property: X;
}

// in another file:
import { X, A } from 'a';
import { B } from 'b';

const x: A = { property: X.A };
const y: B = x; // Works fine, as long as the name of the enums are the same, and the keys in the source are wider or equal to the target.
```

One funny example to top it off:

```ts
// In package A
enum X {
    A = 'SOMEVALUEA',
    B = 'SOMEVALUEB',
    C = 'SOMEVALUEC',
}
interface A {
    property: X;
}

// In package B
enum X {
    A = 'A',
    B = 'B',
    C = 'C',
}
interface B {
    property: X;
}

// in another file:
import { X, A } from 'a';
import { B } from 'b';

const x: A = { property: X.A };
const y: B = x; // Works fine??? :)
console.log(y.property); // prints SOMEVALUEA, which is not assignable to any value in b.X
```

### Solution

In case of an `enum` with `string` backing values, we recently switched to the following construct:

```ts
enum X {
    A = 'A',
    B = 'B',
    C = 'C',
}
interface A {
    property: `${X}`; // 'A' | 'B' | 'C'
}
```

If you use `${X}` as a property type but also expose the enum `X`, you can still use `X.A` for assigning values to the property, but you do not have any issues with assignability as long as the values are comparable.
