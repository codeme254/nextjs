# layouts

A layout is UI that is shared between multiple pages in the app.

Layouts are a feature used to define reusable UI components that persist across multiple pages or parts of your 
application.

They are perfect for shared elements like headers, footers, navigation bars, and general page structure.

## Important things to know about layouts
1. **Layouts are defined in the `app` directory**: Layouts are special components defined in a `layout.jsx` 
(or `layout.tsx` for TypeScript) file within a folder inside the app directory.
2. **Hierarchical and nested layouts**: Next.js layouts follow a hierarchical structure, meaning layouts can be nested 
to create unique structures for different parts of your application.
3. **Server-Side by default**: Layouts, like other components in the App Router, are rendered on the server by default 
for better performance and SEO.
4. **Persistence across routes**: Layouts persist between route changes, so components like navigation bars or 
sidebars don’t re-render unnecessarily.
5. **Nested layouts**: You can define layouts within layouts for more granular control.
6. **Shared state or context**: Use layouts to provide shared context (e.g., theme or user data) to child components.

## The Root Layout
The Root layout is the top most layout and is a mandatory layout for every next.js application.

The Root Layout MUST always have the `html` and the `body` tags.

Every layout component (including the root layout) should accept a children prop that will be replaced by a child
component during rendering.

```TypeScript
export default function RootLayout(
  { children }: { children: React.ReactNode}
) 
{

}
```

Below is a complete root layout, all other layouts take the same shape except that we don't have to repeat
the `html` tag and the `body` tag:

```TypeScript
export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html>
      <body>
        <header>This is the header of the site</header>
        {children}
        <footer>This is the footer of the site</footer>
      </body>
    </html>
  );
}
```

## Nested layouts
Layouts can be nested, which means we can create a layout specifically for about page, product page e.t.c.

Let's create a layout specifically for the about page

```TypeScript
// src/app/about/layout.tsx
export default function Layout({ children }: { children: React.ReactNode }) {
  return (
    <>
      <h2>about page layout</h2>
      {children}
    </>
  );
}
```

As seen in the example above, we don't have to define the `html` and `body` tags in this layout since it is
not the root layout, also, we don't have to repeat the header and the footer here since they are already defined
in the root layout.