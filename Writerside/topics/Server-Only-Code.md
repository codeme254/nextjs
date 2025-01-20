# Server-Only Code

When building next.js apps, certain code is intended to execute only on the server.

You might have modules or functions that use multiple libraries, use environment variables, interact directly with a 
database, or process confidential information.

Since JavaScript modules can be shared, it is possible for code that is meant only for the server to end up in the 
client.

If server-side code gets bundled into the client-side JavaScript, it could lead to a bloated bundle size, expose
security keys, database queries, and sensitive business logic.

It is crucial to separate server-only code from client-side code to protect the application's security and integrity.

## The `server-only` package.
This package provides a build-time error if developers accidentally import one of these modules into a client
component.

To install the package:

```Bash
npm i server-only
```

When we import this package in a module, it ensures the module causes a build-time error when included in a client
bundle.

```TypeScript
// src/utils/server-utils.ts
import "server-only";

export const myServerSideFunction = () => {
  console.log(`Server Side Code`);
  return "server result";
};
```

The client component below will throw an error because we are using a function that should be server-only:
```TypeScript
// src/app/server-route/page.tsx
"use client";
import { myServerSideFunction } from "@/utils/server-utils";
export default function ClientComponent() {
  const result = myServerSideFunction();
  return <h1>Client component - {result}</h1>;
}
```
![server only error](server-only-error.png)