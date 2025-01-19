# Dynamic Rendering

Dynamic rendering is a server rendering strategy where routes are rendered for each user at request time.

It is useful when a route has data that is personalized to the user or contains information that can only be known
at request time, such as cookies in the URL's search parameters.

Dynamic rendering can be used in news websites, personalized e-commerce pages, and social media feeds.

## How to dynamically render.
During rendering, if a dynamic function is discovered, Next.js will switch to dynamically rendering the whole route.

In Next.js, these dynamic functions include: cookies(), headers(), and searchParams.

Using any of these will opt the whole route into dynamic rendering.