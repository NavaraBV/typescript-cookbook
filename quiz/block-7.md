## Decorators
### On what can you apply a decorator?

    1. Classes and methods
    2. Classes, methods and properties
    3. Classes, methods, properties, accessors and parameters
    4. Classes, methods, properties, accessors, parameters and functions

Answer: 3.

See also https://saul-mirone.github.io/a-complete-guide-to-typescript-decorator/

### For what can you use decorators (multiple choice)
    1. Extends/mutate classes with new methods and properties
    2. Add metadata on classes, methods, properties, accessors and paramters
    3. You should not use decorators since they are experimental

We can then discuss about decorators and use the examples of
* angular decorators
* class-validator & class-transform decorators

#### Example class-transform class-validator
```ts
export default class GetNextBestOfferResponse {
  @IsDefined()
  @IsUUID()
  public profileId!: string;

  @IsDefined()
  @IsIn(Object.values(Brand))
  public brand!: Brand;

  @IsDefined()
  @IsIn(Object.values(Channel))
  public channel!: Channel;

  @IsDefined()
  @ValidateNested({ each: true })
  @Type(() => GetNextBestOfferResponseOffer)
  public offers!: GetNextBestOfferResponseOffer[];

  static fromJSON(src: string): GetNextBestOfferResponse {
    return transformAndValidateSync(GetNextBestOfferResponse, src) as GetNextBestOfferResponse;
  }
}
```