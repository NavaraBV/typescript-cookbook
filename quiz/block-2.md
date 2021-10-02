## Type guards
### What is the correct way to type guard errors in `try {} catch (e) {}` constructions

    1.

```ts
// 1
try {
    doSomething();
} catch(e: HttpError) {
    // e is of type A
} catch(e: any) {
    // all other errors
}

// 2
try {
    doSomething();
} catch (e: unknown) {
    if (e instanceof HttpError) {
    } else if (e instanceof InternalError) {
    } else {
    }
}
```

Answer: 2.

## Type narrowing

### Given the following code, what is the type of contract at the point of `A`:

    ```ts
    interface Contract {
        name: string;
        duration: number;
        phonenumber: string;
    }

    interface VgzContract extends Contract {
        name: 'vgz';
        website: string;
    }

    function getcontact(contract: Contract | VgzContract) {
        if (contract.name === 'vgz') {
            // A
        } else {
            // B
        }
    }
    ```

    1. `Contract`
    2. `VgzContract`
    3. `Contract | VgzContract`

Answer: 3.

## Exhaustive checks

### Gegeven het volgende type `type CarContract = VgzContract | CZContract`. Hoe kan je een exhaustiveness check inbouwen? In andere woorden, hoe zorg je ervoor dat Typescript een compile error krijgt wanneer je `CarContract` verbreed met `type CarContract = VgzContract | CZContract | ANWBContract`:

    1. Niet mogelijk in de huidige versie van TypeScript
    2.

    ```ts
    // 2
    function getContact(contract: CarContract) {
        switch (contract.name) {
            case 'cz':
                return contract.phonenumber;
            case 'vgz':
                return contract.website;
            default:
                const _exhaustiveCheck: never = '';
                return _exhaustiveCheck;
        }
    }

    // 3  
    function getContact(contract: CarContract) {
        if (contract.name === 'cz') {
            return contract.phonenumber;
        } else if (contract.name === 'vgz') {
            return contract.website;
        }
        const _exhaustiveCheck: never = '';
        return _exhaustiveCheck;
    }
    ```

    4. Zowel 2. als 3.

Answer: 4.