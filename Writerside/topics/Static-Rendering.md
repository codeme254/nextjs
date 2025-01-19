# Static Rendering

Static rendering is a server rendering strategy where we generate HTML pages at the time of building our application.

This approach allows the page to be built once, cached by a CDN, and served to the client almost instantly.

Static rendering is particularly useful for blog pages, e-commerce product pages, documentation and marketing pages.

## How to statically render.
Static rendering is the default rendering strategy in the app router.

This means that all routes are automatically prepared at build time without additional setup.

However, it is important to note that in development mode, a page will be pre-rendered for every request, this is to
improve developer experience.