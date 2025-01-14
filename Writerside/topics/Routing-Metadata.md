# Routing Metadata

Routing metadata refers to the meta-information associated with routes, which helps in configuring and optimizing 
your application's behavior, SEO, and social media previews.

Routing metadata is important for ensuring Search Engine Optimization (SEO) which enhances visibility and 
attracting users.

Next.js introduced the Metadata API which allows you to define metadata for each page ensuring accurate and relevant
information is displayed when your pages are shared or indexed.

## Configuring metadata
With the app router, there are two ways of configuring metadata:
- Export a static metadata object.
- Export a dynamic `generateMetadata` function.

### Exporting a static metadata object
Let's create metadata for the about page, we can export an object called `metadata` from `/about/page.tsx` or 
`/about/layout.tsx`, let's start with `/about/page.tsx`:

```TypeScript
// src/app/about/page.tsx
export const metadata = {
  title: "about our company",
  description: "a detailed look at us to know us better",
};

export default function AboutPage() {
  return <h1>About page</h1>;
}
```

### Export a dynamic `generateMetadata` function
Dynamic metadata depends on dynamic information such as the current route parameters, external data, metadata in
parent segments among others.

To define dynamic metadata, we export a `generateMetadata` function that returns a metadata object from a `layout.tsx`
or `page.tsx` file.

In our example, we have a `/products/[productId]` page, but currently, all products get rendered with the same title,
we would like to have a unique title for each product, in our case, we would like the `productId` to be part of the
page title making it dynamic.

```TypeScript
// src/app/products/[productId]/page.tsx
import { Metadata } from "next";
import { notFound } from "next/navigation";
const products = [
  { id: 1, name: "laptop" },
  { id: 2, name: "phone" },
  { id: 3, name: "monitor" },
];

type Props = {
  params: {
    productId: string;
  };
};

// ===================generateMetadata function===============
export function generateMetadata({ params }: Props): Metadata {
  const product = products.find((product) => product.id === +params.productId);
  return {
    title: `Product ${params.productId}`,
    description: product?.name,
  };
}

export default async function Product({ params }: Props) {
  const { productId } = await params;
  const product = products.find((product) => product.id === +productId);
  if (!product) {
    notFound();
  }
  return (
    <>
      <h2>Products with {productId}</h2>
    </>
  );
}
```

The generate metadata function can also be defined as an async function:

## Metadata rules
Both `layout.tsx` and `page.tsx` files can export metadata. If defined in a layout, it applies to all pages in that
layout, but if defined in a page, it applies only to that page.

Metadata is read in order from the root level down to the final page level.

When there's metadata in multiple places for the same route, they get combined, but page metadata will replace 
layout metadata if they have the same properties.

You CANNOT export the metadata object and `generateMetadata` function from the same route segment.