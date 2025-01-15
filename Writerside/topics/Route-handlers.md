# Route handlers

We have so far looked at how to route pages.

In Next.js, we can also create custom request handlers for our routes using features called route handlers.

Unlike page routes, which respond with HTML content, route handlers allow you to create RESTful endpoints, giving you
full control over the response.

This is helpful as there is no overhead of having to configure a separate server.

Route handlers are also great for making external API requests.

Route handlers run server-side, ensuring that sensitive information like private keys remain secure and are never
shipped to the browser.

## File structure
Similar to page routes, route handlers must also be placed within the `app` folder.

You define route handlers by creating a special `route.js` (or `route.ts` for TypeScript) file inside a directory under
`app` directory.

### A Basic Example
Here’s an example of a route handler for handling a GET request:

```TypeScript
// src/app/hello/route.ts
export async function GET() {
  return new Response(JSON.stringify({ message: "Hello, World!" }));
}
```

When you navigate to `http://localhost:3000/hello`, the handler will respond with:
```TypeScript
{
  "message": "Hello, World!"
}
```

## Handling GET request
Let's use the following array of users as our fake data:

```TypeScript
// src/app/users/data.ts
const users = [
  {
    id: 1,
    firstName: "john",
    lastName: "doe",
  },
  {
    id: 2,
    firstName: "jane",
    lastName: "doe",
  },
  {
    id: 3,
    firstName: "jack",
    lastName: "smith",
  },
];
```
Get requests are used to retrieve a resource(s).

To handle a get request, export a function called `GET` as shown below:

```TypeScript
// src/app/users/route.ts
import { users } from "./data";

export async function GET() {
  return new Response(JSON.stringify(users));
}
```

## Handling POST request
Post requests are used to create a resource(s).

To handle a POST request, export a function called `POST`.

Every route handler function receives the standard request object as a parameter, from here; we can access the JSON
body specified as part of the request as shown below:

```TypeScript
// src/app/users/route.ts
import { users } from "./data";

export async function POST(request: Request) {
    const user = await request.json();
    const userId = Math.trunc(Math.random()*1000)
    const newUser = {...user, id: userId};
    users.push(newUser);
    return new Response(JSON.stringify(users));
}
```

In case we wanted to customize the status code:

```TypeScript
// src/app/users/route.ts
import { users } from "./data";

export async function POST(request: Request) {
  const user = await request.json();
  const userId = Math.trunc(Math.random() * 1000);
  const newUser = { ...user, id: userId };
  users.push(newUser);
  return new Response(JSON.stringify(users), { status: 201 });
}
```

## Dynamic route handlers (url params)
Dynamic route handlers work similar to dynamic page routes.

Within the users folder, create a new folder called `[id]`.

Inside this folder, create a `route.ts` file.

The following route handler, for example, will return a specific user:

```TypeScript
// src/app/users/[id]/route.ts
import { users } from "../data";
export function GET(request: Request, { params }: { params: { id: string } }) {
  const user = users.find((user) => user.id === +params.id);
  if (user) return new Response(JSON.stringify(user));
  return new Response(JSON.stringify({ message: "User Not Found" }), {
    status: 404,
  });
}
```

The handler function receives two parameters; the first one is the request object that we have already seen, the second
parameter is the context, currently, the only value of context is params, which is an object containing dynamic
route parameters (url params) for the current route.

In the example above, from context, we have destructured params; params in our case is an object that contains the id
route parameter which is of type string.

## Handling a PATCH requests
Patch requests apply partial modifications to a resource.

To handle a Patch request, export a function called `PATCH`

```TypeScript
import { users } from "../data";

export async function PATCH(
  request: Request,
  { params }: { params: { id: string } },
) {
  const newInfo = await request.json();

  const updatedUsers = users.map((user) => {
    if (user.id === +params.id) {
      const updatedUser = { ...user, ...newInfo };
      return updatedUser;
    }
    return user;
  });
  return new Response(JSON.stringify(updatedUsers), { status: 200 });
}
```

## Handling DELETE requests
Delete request deletes a specific resource.

To handle a delete request, export a function called `DELETE`:

```TypeScript
// src/app/users/[id]/route.ts
import { users } from "../data";

export async function DELETE(
  request: Request,
  { params }: { params: { id: string } },
) {
  const remainingUsers = users.filter((user) => user.id !== +params.id);
  return new Response(JSON.stringify(remainingUsers), { status: 200 });
}
```