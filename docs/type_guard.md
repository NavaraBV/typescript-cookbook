## Type guards

TS does not always understand what kind of type narrowing it can apply. To help it along we can use type guards:

```ts
interface PhoneData {
    phoneNumber: string;
}

interface EmailData {
    email: string;
}

type ContactData = PhoneData | EmailData;

const isPhoneData = (contactData: ContactData): contactData is PhoneData => 
    (contactData as PhoneData).phoneNumber !== undefined;

const handleContactData = (contactData: ContactData) => {
    if(isPhoneData(contactData)) {
        // contactData has type PhoneData
        console.log(`Phone number is ${contactData.phoneNumber}`);
    } else {
        // contactData has type EmailData
        console.log(`Email address is ${contactData.email}`);
    }
}
```

Using these typeguards can be very useful in other scenario's as well. Since there is a difference between a `null` and `undefined` value, we often do `!== null && !== undefined` checks. This is quite wordy.
Instead, you can use the following typeguards:

```ts
const isDefined = <T>(value: T | undefined | null): value is T => value !== null && value !== undefined;
const isNullOrUndefined = <T>(value: T | undefined | null): value is null | undefined => value === null || value === undefined;

const printWhenDefined = <T>(value?: T) => {
    if(isDefined(value)) {
        console.log(value); // Value is of type T here
    }
}
const throwErrorWhenUndefined = <T>(value?: T) => {
    if(isNullOrUndefined(value)) {
        throw new Error('value is not defined!');
    }
    // value is of type T from here on.
}
```

Combining this with some tuple `const`s and some variadic typing, we can make some other utils as well:
```ts
export type DefinedArray<T extends readonly any[]> = { [K in keyof T]: NonNullable<T[K]> };
export function isArrayDefined<T extends readonly any[]>(arr: T | undefined | null): arr is DefinedArray<T> {
    return isDefined(arr) && arr.every(isDefined);
}
```

And using these utils, we can come to a mapper which makes sure all inputs are defined, otherwise returns `undefined`:
```ts
export const mapWhenDefined = <V extends readonly any[], R>(...args: [...vals: V, mapperFn: (...vals: DefinedArray<V>) => R]) => {
    const values = args.slice(0, args.length - 1) as any as V;
    const selector = args[args.length - 1] as (...vals: V) => R;
    return isArrayDefined(values) ? selector(...values) : undefined;
}
```