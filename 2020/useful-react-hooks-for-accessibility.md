---
date: 2020-05-19
---
<meta property="og:published_time" content="2020-05-19">

# Useful React hooks for accessibility

```tsx
import { useMemo } from 'react';
import { v4 as uuid } from 'uuid';

export function useLabelledBy(): readonly [
  string,
  { 'aria-labelledby': string }
] {
  const uniqueID = useMemo(uuid, [uuid]);
  return [uniqueID, { 'aria-labelledby': uniqueID }];
}

export function useDescribedBy(): readonly [
  string,
  { 'aria-describedby': string }
] {
  const uniqueID = useMemo(uuid, [uuid]);
  return [uniqueID, { 'aria-describedby': uniqueID }];
}
```

This allows us to create `id`/`aria-labelledby` pairs to add as attributes to the label and labelled. And another hook for descriptions with `id`/`aria-describedby` attributes.

```tsx
interface ProductProps {
  name: string;
  description: string;
}
function Product({ name, description, price }: ProductProps) {
  const [labelID, labelledBy] = useLabelledBy();
  const [descriptionID, describedBy] = useDescribedBy();

  return (
    <article {...labelledBy} {...describedBy}>
      <h2 id={labelID}>{name}</h2>
      <p id={descriptionID}>{description}</h2>
      ...
    </article>
  );
}
```

And then for `<dl>` which are useful for presenting key-value pairs (e.g. attributes of a product, FAQ questions and answers). Here’s a helper that creates a `<dt>` and `<dd>` pair and associates them so they label one another.

```tsx
import { visuallyHidden } from "./shared.css";

interface TermAndDefinitionProps {
  term: React.ReactNode;
  definition: React.ReactNode;
  termVisuallyHidden?: boolean;
}
function TermAndDefinition(props: TermAndDefinitionProps): JSX.Element {
  const [termID, labelledby] = useLabelledBy();  return (
    <>
      <dt
        id={termID}
        className={props.termVisuallyHidden ? visuallyHidden : undefined}
      >
        {props.term}
      </dt>
      <dd {...labelledby}>{props.definition}</dd>
    </>
  );
}
```

We could use it like so to present the price and color for our product:

```tsx
interface ProductProps {
  name: string;
  description: string;
  price: string;
  color: string;
}
function Product({ name, description, price, color }: ProductProps) {
  const [labelID, labelledBy] = useLabelledBy();
  const [descriptionID, describedBy] = useDescribedBy();

  return (
    <article {...labelledBy} {...describedBy}>
      <h2 id={labelID}>{name}</h2>
      <p id={descriptionID}>{description}</h2>
      <dl>
        <TermAndDefinition term="Price:" definition={price} />
        <TermAndDefinition term="Color:" definition={color} />
      </dl>
    </article>
  );
}
```

This means in our tests we can look up the value for a specific key in the UI. We could use [React Testing Library](https://testing-library.com/docs/react-testing-library/intro) which offers looking elements up their accessible role.

The implicit role for a `<dd>` is definition, so we can look those up by their accessible name. We have wired the corresponding <dt> to be the label which becomes the accessible name. So testing becomes straightforward.

Say to assert that the price shown is $50:

```ts
expect(
  screen.getByRole('definition', { name: 'Price:' })
).toHaveTextContent('$50');
```

Or the color is red:

```ts
expect(
  screen.getByRole('definition', { name: 'Color:' })
).toHaveTextContent('Red');
```
