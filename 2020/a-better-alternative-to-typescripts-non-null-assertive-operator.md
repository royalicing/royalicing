---
date: 2020-05-14
---
<meta property="og:published_time" content="2020-05-14">

# A better alternative to TypeScript’s non-null assertive operator

TypeScript supports using `!` for telling the compiler that the value is not null or undefined. This is known as the _non-null assertive operator_.

user.type(document.activeElement!, "Some text");
However, it is simply stripped out when transpiling to JavaScript. No check is done at runtime. So, unlike the name suggests, we are not actually _asserting_ anything. It would be better, if we are 100% confident that the value is not null/undefined, to have an additional runtime check that blows up if somehow we were mistaken.

This also solves eslint’s `no-non-null-assertion` rule, which might be enabled in your project by default if you use a preset pack of lint rules.

For Jest tests, you could implement it as:

```ts
export function assertDefined<T>(
  value: T | null | undefined
): asserts value is T {
  expect(value).not.toEqual(null);
}
```

For implementation code:

```ts
export function assertDefined<T>(
  value: T | null | undefined
): asserts value is T {
  if (value == null) {
    throw new Error(`Fatal error: value ${value} must not be null/undefined.`);
  }
}
```

We can now remove our usage of the non-null assertive operator, and replace it with our assertDefined() function. Here the first example of a Jest test:

```ts
assertDefined(document.activeElement);
user.type(document.activeElement, "Some text");
```
