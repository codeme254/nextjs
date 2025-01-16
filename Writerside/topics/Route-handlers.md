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

## Query parameters
Query parameters are key-value pairs appended to the end of a URL to send additional information to the server.

They are commonly used to filter, sort, or pass data in a web request without changing the resource path.

Query parameters follow a `?` in the url.

Multiple query parameters are separated by `&`.

Each parameter is written as a key-value pair.

To handle a query parameter in Next.js, you need the request parameter.

So far, we have been defining the request type as the standard request api. However, in the context of Next.js, the
type we should actually be dealing with is `NextRequest` which is imported form `next/server`.

To access all the query parameters, you use `request.nextUrl.searchParams`

```TypeScript
// src/app/users/route.ts
import { users } from "./data";
import { NextRequest } from "next/server";

export async function GET(request: NextRequest) {
  const searchParams = request.nextUrl.searchParams;
  const gender = searchParams.get("gender");

  if (gender) {
    const filteredUsers = users.filter((user) => user.gender === gender);
    return new Response(JSON.stringify(filteredUsers));
  }
  return new Response(JSON.stringify(users));
}
```

Try the example above with `http://localhost:3000/users?gender=male`

## Headers in route handlers.
HTTP headers represent the metadata associated with an API request and response.

This metadata can be classified into two categories:
- Request Headers
- Response Headers

### Request Headers
These are sent by a client such as a web browser to the server. They contain essential information about the request,
which helps the server understand and process it correctly.

For example, the `User-Agent` which identifies the browser and operating system to the server. `Accept` header
indicates the content like text, video or image formats that the client can process. `Authentication` header is used
by the client to authenticate itself to the server.

Below is how you can extract request headers in Next.js:

```TypeScript
// src/app/hello/route.ts
import { NextRequest } from 'next/server';

export async function GET(request: NextRequest) {
  const requestHeaders = new Headers(request.headers);
  console.log(requestHeaders.get("User-Agent"))
  return new Response(JSON.stringify({ message: "Hello, World!" }));
}
```

You can also choose to use the `headers` function provided by Next.js which is imported from `next/headers`
as shown below.

```TypeScript
// src/app/hello/route.ts
import { NextRequest } from 'next/server';
import { headers } from 'next/headers';

export async function GET(request: NextRequest) {
  const headersList = await headers();
  console.log(headersList.get("User-Agent"))
  return new Response(JSON.stringify({ message: "Hello, World!" }));
}
```

### Response Headers
Response headers are sent back from the server to the client.

They provide information about the server and the data being sent in the response.

An example is the `Content-Type` header which indicates the media type of the response. It tells the client what the
data type of the returned content is, such as text/html for HTML documents, application/json for JSON data e.t.c

To set headers, you need to return a response with the new headers.

Below is how you can set outgoing headers:
```TypeScript
// src/app/users/route.ts
import { users } from "./data";
import { NextRequest } from "next/server";

export async function GET(request: NextRequest) {
  const searchParams = request.nextUrl.searchParams;
  const gender = searchParams.get("gender");

  if (gender) {
    const filteredUsers = users.filter((user) => user.gender === gender);
    return new Response(JSON.stringify(filteredUsers), {
      headers: {
        "Content-Type": "application/json"
      }
    });
  }
  return new Response(JSON.stringify(users), {
    headers: {
      "Content-Type": "application/json"
    }
  });
}
```

## Cookies in Route Handlers
Cookies are small pieces of data that a server sends to a user's web browser.

The browser may store the cookie and send it back to the same browser with later requests.

Cookies are mainly used for three purposes:
- Session management like logins and shopping carts.
- Personalization like user preferences and themes.
- Tracking like recording and analyzing user behavior.

### Setting cookies

#### Method 1: return a response using the `Set-Cookie` header.

```TypeScript
// src/app/users/route.ts
import { users } from "./data";
import { NextRequest } from "next/server";

export async function GET(request: NextRequest) {
  const searchParams = request.nextUrl.searchParams;
  const gender = searchParams.get("gender");

  if (gender) {
    const filteredUsers = users.filter((user) => user.gender === gender);
    return new Response(JSON.stringify(filteredUsers), {
      headers: {
        "Content-Type": "application/json"
      }
    });
  }
  return new Response(JSON.stringify(users), {
    headers: {
      "Content-Type": "application/json",
      "Set-Cookie": "authToken=123456" // cookieKey=cookieValue
    }
  });
}
```

#### Method 2: using the cookies function provided by Next.js
We can use the `cookies` function provided by next.js which is imported from 'next/headers'

```TypeScript
// src/app/users/route.ts
import { users } from "./data";
import { NextRequest } from "next/server";
import { cookies } from 'next/headers';

export async function GET(request: NextRequest) {
  const cookiesFn = await cookies()
  cookiesFn.set("authToken", "xyz123$abc")

  return new Response(JSON.stringify(users), {
    headers: {
      "Content-Type": "application/json"
    }
  });
}
```

### Getting Cookies
#### Method 1: using the request parameter

```TypeScript
// src/app/users/route.ts
import { users } from "./data";
import { NextRequest } from "next/server";

export async function GET(request: NextRequest) {
  const authToken = request.cookies.get("authToken");
  console.log(authToken);
  
  return new Response(JSON.stringify(users), {
    headers: {
      "Content-Type": "application/json",
      "Set-Cookie": "authToken=123456"
    }
  });
}
```

#### Method 2: using the cookies function provided by Next.js to get cookies
We can use the `cookies` function provided by next.js which is imported from 'next/headers':

```TypeScript
// src/app/users/route.ts
import { users } from "./data";
import { NextRequest } from "next/server";
import { cookies } from 'next/headers';

export async function GET(request: NextRequest) {
  const cookiesFn = await cookies()
  console.log(cookiesFn.get("authToken"))

  return new Response(JSON.stringify(users), {
    headers: {
      "Content-Type": "application/json"
    }
  });
}
```

The cookies() function supports other methods such as delete, has, e.t.c.

## Middleware
Middleware offers a way to intercept and control the flow of requests and responses within your applications.

It does this at a global level significantly enhancing features like redirection, URL rewrites, authentication,
headers and cookies management, and more.

To create a middleware in Next.js, start by adding a `middleware.ts` file in the source folder.

Middlewares in Next.js allows us to specify paths where it will be active.

There are two approaches to achieve this:
- Custom matcher config
- Conditional statements

In the following example, we are using the custom matcher config to specify that this middleware should be executed
before the request hits the users route

```TypeScript
export function middleware() {
    console.log("Middleware executed...");
}

// configuring the middleware using matcher config

export const config = {
    matcher: ["/users"]
}
```