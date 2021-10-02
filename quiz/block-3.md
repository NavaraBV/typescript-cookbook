## Enums
### What determines assignability of one enum to another?
    1. Values of the enum
    2. Name and values of the enum
    3. Name and keys of the enum
    4. Name, values and keys of the enum

Answer: 3.

###  Does the following compile as of TS4.1?:
    ```ts
    enum A {
        A = 'A',
        B = 'B',
        C = 'C'
    }
    type AValueUnion = `${A}`; // 'A' | 'B' | 'C'
    const switchFn = (value: AValueUnion): string => {
        switch(value) {
            case A.A:
                return 'A';
            case A.B:
                return 'B';
            case A.C:
                return 'C';
        }
    }
    ```
    1. Yes
    2. No
    3. It should

Answer: 2. and 3., but that last one's debatable :P

### How do you get the values of the following enum:

    ```ts
    enum Gender {
        FLUID = 'fluid',
        MALE = 'male',
        FEMALE = 'female',
    }
    ```

    1. You can't, they only exist compile time

    2.

    ```ts
    Object.values(Gender);
    ```

    3.

    ```ts
    Gender.values();
    ```

Answer: 2.