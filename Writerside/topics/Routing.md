# Routing

Next.js uses a file-system-based routing mechanism.

URL paths that users can access in your browser are defined by files and folders in your code.

## Routing conventions
1. All routes must be placed inside the `app` folder.
2. Every file that corresponds to a route must be named `page.js` or `page.tsx` depending on whether TypeScript is being
used or not.
3. Each folder corresponds to a path segment in the browser URL.

Throughout this guide, we will be using TypeScript.

## Static routes
### Let's implement a home page

Let's add a route that should be rendered when a user visits our websites (home route).

To do this, create a `page.tsx` in the app folder (`/app/page.tsx`) and export a React component.

```TypeScript
// src/app/page.tsx
export default function Home() {
  return <h1>Home Page</h1>;
}
```

### Let's create an about page
To create an about page, create a folder called `about` inside the `app` folder.

This will now correspond to the `/about` endpoint.

Inside `about` folder, create a `page.tsx` file and from there, export a React component, this component will be
rendered when the user visits the `/about` endpoint.

```TypeScript
// src/app/about/page.tsx
export default function AboutPage() {
  return <h1>About page</h1>;
}
```

## static nested routes
Assuming we want `/about/company` page and `/about/staff` page:

Create a folder called `company` within the `about` folder.

Inside `about/company/` folder, create a file called `page.tsx` and export a React component, this React component
will be rendered when our users visit `/about/company/` endpoint.

```TypeScript
// src/app/about/company/page.tsx
export default function CompanyPage() {
  return <h2>About our company</h2>;
}
```

Let's do the same for `/about/staff`
```TypeScript
// src/app/about/staff/page.tsx
export default function StaffPage() {
  return <h2>This is the staff page</h2>;
}
```

## Dynamic Routes
Dynamic routes in Next.js allow you to create routes with variable segments in their URL structure.

These are useful for building pages where the content or structure depends on parameters such as IDs, slugs, or 
other data values.

Let's say we want to create a page for each staff member, and visitors should be able to navigate to URLs like:
`/about/staff/johndoe`
to view a page specifically for johndoe.

We could create individual folders for each staff member, each containing its own page.tsx file, like this:

```Text
app/
├── about/
│   ├── staff/
│   │   ├── johndoe/
│   │   │   ├── page.tsx
│   │   ├── janedoe/
│   │   │   ├── page.tsx
│   │   ├── mike/
│   │   │   ├── page.tsx

```
However, this approach is not scalable, especially if we have a large number of staff members or frequently 
need to add or remove staff.

This is where dynamic routes come in.

With dynamic routes, you can create a single file to handle all the staff pages dynamically. 

To create a dynamic route, simply enclose the folder name in brackets.

In our case, let's create a folder called `[name]` inside `/about/staff/` folder, inside this folder, create a `page.tsx`
and export a React component.

```TypeScript
// src/app/about/staff/[name]/page.tsx

export default function Staff() {
  return (
    <>
      <h2>A specific staff member</h2>
    </>
  );
}
```

In a typical application, you would want to extract the dynamic parameter and use it to perform an action such as
an api call, here is how to achieve this:

```TypeScript
// src/app/about/staff/[name]/page.tsx

export default async function Staff({ params }: { params: { name: string } }) {
  const { name } = await params;
  return (
    <>
      <h2>A specific staff member ({name})</h2>
    </>
  );
}
```

Every page in the app router receives route parameters as a prop and in the code above, we are destructing it as
params. The params object contains the route parameters as key value pairs.

## Nested dynamic routes
sometimes, multiple dynamic segments are required.

For example, each of our staf might have multiple reviews, and we might want a page for each of these reviews, for
example, visiting `/about/staff/johndoe/reviews/1` should show the review with id 1.

```TypeScript
// src/app/about/staff/[name]/reviews/[reviewId]/page.tsx

export default async function ReviewPage({
  params,
}: {
  params: { name: string; reviewId: string };
}) {
  const { name, reviewId } = await params;
  return (
    <>
      <h2>
        Review with id {reviewId} for staff {name}
      </h2>
    </>
  );
}
```

## Catch all segments
We can define one file that handles all the route segments in the url.

Catch-all segments are defined by wrapping a dynamic segment in square brackets (`[]`) and prefixing it
with three dots(`...`).

Consider the example below:

```TypeScript
// src/app/docs/[...slug]/page.tsx
export default function Docs() {
  return (
    <>
      <h1>Docs Page</h1>
    </>
  );
}
```

The example above will match any of the following routes:
- /docs/feature1/concept1
- /docs/feature2/concept2/example1
- /docs/features/feature3/concept1/example4
- e.t.c

To access the different segments, you still rely on the params object provided by Next only that this time,
params is an object with a slug field that is an array of strings.

```TypeScript
// src/app/docs/[...slug]/page.tsx
export default async function Docs({ params }: { params: { slug: string[] } }) {
  const { slug } = await params;
  return (
    <>
      <h1>Docs Page</h1>
      {slug.map((item, i) => (
        <p key={i}>{item}</p>
      ))}
    </>
  );
}
```

## Custom not found page
To create a custom not found page, simply create a file `not-found.tsx` or `not-found.jsx` in the app folder
```TypeScript
// src/app/not-found.tsx
export default function NotFound() {
  return <h1>Sorry, we don't have that page</h1>;
}
```
### Rendering the not-found page programmatically
We can also programmatically render the not found page based on a certain condition.

For this purpose, we can use the `notFound()` function from Next as shown below.

```TypeScript
// src/app/products/[productId]/page.tsx
import { notFound } from "next/navigation";
const products = [
  { id: 1, name: "laptop" },
  { id: 2, name: "phone" },
  { id: 3, name: "monitor" },
];

export default async function Product({
  params,
}: {
  params: { productId: string };
}) {
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

Not found pages can also be defined at the folder level, for example, you can have a custom
`/about/not-found.tsx`

## Private folders
A private folder indicates that it is a private implementation detail and should not be considered
by the routing system.

This means that the folder and all its subfolders are excluded from routing.

To create a private folder, simply prefix the folder with an underscore (_).

```TypeScript
// src/app/_lib/formatDate.ts
export default function formatDate() {
    return "format date."
}
```

Private folder can be used for the following:
- Separating UI logic from routing logic.
- Consistently organizing internal files across a project.
- Sorting and grouping files in code editors.
- Avoiding potential naming conflicts with future Next.js file conventions.

If your url is supposed to have an underscore prefixed e.g., /about/_products, then you can prefix
the folder name with '%5F' which is the URL-encoded form of an underscore.

The page below for example will be accessed through `/_extras`

```TypeScript
// src/app/%5Fextras/page.tsx
export default function Extras() {
  return <h2>Appendix page</h2>
}
```

## Route groups
This feature allows us to group our routes and project files without affecting the URL path structure.

Let's implement some auth routes, we will create routes for register, login and forgot password.

Forgot password:

```TypeScript
// src/app/forgot-password
export default function ForgotPassword() {
    return <h2>Reset your password</h2>
}
```

Login:

```TypeScript
// src/app/login/page.tsx
export default function Login() {
    return <h2>Login to your account</h2>
}
```

Register:

```TypeScript
// src/app/register/page.tsx
export default function Register() {
    return <h2>Create an account</h2>
}
```

However, with the current set-up, the authentication routes are scattered all over the place, it could
be nice to organize them into a single folder, but this would mean restructuring the urls, and sometimes
we might not want that.

This is where route groups come in, the feature allows us to group our routes and project files without 
affecting the URL path structure.

To define a route group, wrap the folder name with parentheses.

Create a folder called `(auth)` and move all the three routes above to this folder.

```TypeScript
// src/app/(auth)/forgot-password
export default function ForgotPassword() {
    return <h2>Reset your password</h2>
}
```

```TypeScript
// src/app/(auth)/login/page.tsx
export default function Login() {
    return <h2>Login to your account</h2>
}
```

```TypeScript
// src/app/(auth)/register/page.tsx
export default function Register() {
    return <h2>Create an account</h2>
}
```