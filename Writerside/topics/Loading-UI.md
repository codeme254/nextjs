# Loading UI

Loading the user interface is essential for providing feedback to the user while your application fetches data or
renders components.

## The `loading.tsx` file
This file allows us to create loading states that are displayed to users while a specific route segment's content is
loading.

The loading state appears immediately upon navigation, giving users the assurance that the application is responsive
and actively loading content.

```TypeScript
// src/app/docs/loading.tsx
export default function Loading() {
  return <h2>Loading the docs page...</h2>;
}
```

With loading.tsx:
1. You can display the loading state as soon as a user navigates to a new route. The immediate feedback reassures the
users that their action has been acknowledged, reduces perceived loading times, and makes the application to be more
responsive.
2. Next.js allows the creation of shared layouts that remain interactive while new route segments are loading, this
means users can continue interacting with certain parts of the applications even if the main content is still being 
fetched.