# Navigation

Users rely on UI elements like links to navigate by either:
- Clicking on them or
- Through programmatic navigation after completing an action


## Link component navigation
To enable client-side navigation, Next.js provides us with the `Link` component.

The `<Link>` component is a React component that extends the HTML `<a>` element, and it's the primary way to navigate
between routes in Next.js

To use it, we need to import it from `"next/link"`.

```TypeScript
// src/app/page.tsx
import Link from "next/link";

export default function Home() {
  return (
    <>
      <Link href="/about">About</Link>
      <Link href="/docs">Docs</Link>
      <Link href="/products">Products</Link>
    </>
  );
}
```

## Active links
In real world scenarios, it is common to style differently the link that corresponds to the currently active route;
this helps the users know which page they are currently at.

To determine if a link is active, Next.js provides the `usePathname` hook from `'next/navigation'`.

Since this is a hook, it means that the component should be a client component, to convert the server component
to a client component, add the `"use client"` directive at the top of the component file.

```TypeScript
"use client";
import Link from "next/link";
import { usePathname } from "next/navigation";
export default function Header() {
  const navLinks = [
    { href: "/about", name: "About" },
    { href: "/docs", name: "Docs" },
    { href: "/products", name: "products" },
  ];

  const pathName = usePathname();
  return (
    <header>
      {navLinks.map((link) => {
        const isActive = pathName.startsWith(link.href);
        return (
          <Link
            href={link.href}
            key={link.name}
            className={isActive ? "font-bold text-red-800" : "text-blue-300"}
          >
            {link.name}
          </Link>
        );
      })}
    </header>
  );
}
```

## Navigating programmatically
Sometimes, we want to navigate users programmatically, for example, when users provide the correct username and
password; you programmatically navigate them to the next page.

This is achieved using the `useRouter` hook from `'next/navigation'`.

```TypeScript
"use client";
import { useRouter } from "next/navigation";

export default function ProductsPage() {
  const router = useRouter();
  function handlePlaceOrder() {
    console.log("Order placed");
    router.push("/");
  }
  return (
    <>
      <h1>Products page</h1>
      <button onClick={handlePlaceOrder}>place order</button>
    </>
  );
}
```
