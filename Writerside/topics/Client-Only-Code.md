# Client-Only Code

Just as it is important to restrict certain operations to the server, it is also equally important to confine
some functionalities to the client.

Client-only code typically interacts with browser-specific features like the DOM, the window object, e.t.c.

Ensuring that such code is executed only on the client side prevents errors during server-side rendering.

## The `client-only` package.
To prevent unintended server side usage of client side code, we can use a package called client-only.

Similar to the `server-only` package, the only thing you need to do is to import the `client-only` package and the
component will throw an error when we try to use it in the server.