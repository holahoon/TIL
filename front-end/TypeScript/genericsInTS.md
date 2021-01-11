# Generics in TypeScript

#### [reference](https://levelup.gitconnected.com/typescript-generics-with-a-witcher-6f449f253e13)

Benefits of using generics in TS help us to create more reusable chunks of code. They help us to create well defined classes, interfaces and function while still giving us the flexibility of keeping them "generic" - by making them work according to a passed type (or types) by the programmer.

## Example

```ts
function iTakeAnyArrays(arr: any[]): any[] {
  return arr;
}
iTakeAnyArrays('here'); // error
```
Above code allows any types of array `arr` and returns it. If anything that's not a type of array will throw an error.
> error: Argument of type 'number' is not assignable to parameter of type 'any[]'

```ts
function iTakeGenericArrays<T>(arr: T[]): T[] {
  return arr;
}
iTakeGenericArrays('again'); // error
```
Above code pretty much does the same thing as `iTakeAnyArrays()`, but it allows the user to define the type automatically. `<T>` is kind of a placeholder which will be replaced by a type passed in by the user.
> error: Argument of type 'number' is not assignable to parameter of type 'unknown[]'

```ts
iTakeAnyArrays([1, 2, 3]);
iTakeGenericArrays(['a', 'b', 'c']);
```
Now the errors are gone because we passed in the correct types properly.
If we hover over the `iTakeGenericArrays`, it shows the following message
> function iTakeGenericArrays<string>(arr: string[]): string[]

