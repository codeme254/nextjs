# Error handling

Handling errors is an important part of any application.

In Next.js, we can use the `error.tsx` file to handle errors.

## The `error.tsx` file
This file is important for the following reasons:
- Creates an error UI tailored to specific segments using the file-system hierarchy to adjust granularity.
- Isolate errors to affected segments while keeping the rest of the application running.
- Adding functionality to attempt to recover from errors without a full page reload.
- Automatically wrap a route segment and its nested children in a React Error boundary.


**NOTE 1**: The component exported from the `error.tsx` file MUST be a client component.

**NOTE 2**: The component exported from the `error.tsx` file takes an error prop of type `Error`.

```TypeScript
// src/app/products/[productId]/page.tsx
export default async function Product({ params }: Props) {
  const { productId } = await params;
  if (+productId > 100) {
    throw new Error("Invalid product id")
  }
  return (
    <>
      <h2>Product with id {productId}</h2>
    </>
  );
}
```

To handle the error thrown from the component above, we can use the `error.tsx` file inside 
`src/app/products/[productId]/` as shown below:

```TypeScript
//src/app/products/[productId]/error.tsx
"use client";

export default function ProductIdError({ error }: { error: Error }) {
  return <h2>{error.message}</h2>;
}
```
